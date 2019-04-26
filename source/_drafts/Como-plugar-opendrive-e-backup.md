```
$ lsusb
…
Bus 001 Device 078: ID 1058:07a8 Western Digital Technologies, Inc.
…
```
ddd

```
Bus 001 Device 008: ID 13fe:1f00 Kingston Technology Company Inc. DataTraveler 2.0 4GB Flash Drive / Patriot Xporter 32GB (PEF32GUSB) Flash Drive
```
idVendor: 13fe
idProduct: 1f00


We’ll use udev to make it happen automatically upon plugging in the drive. It expects instructions in the /etc/udev/rules.d directory, so let’s add a rule file named backup-drive.rules:

```
ENV{ID_FS_LABEL_ENC}=="?*", ACTION=="add", SUBSYSTEMS=="usb", \
    ATTRS{idVendor}=="1058", ATTRS{idProduct}=="0a78", \
    RUN+="/home/$(whoami)/bin/sync-backup-drive-wrapper.sh %k"
```


It ensures sync-backup-drive-wrapper.sh gets called as soon as the USB drive is plugged in.



%k tells udev to pass the device name to the script as an argument. Let’s take a look at sync-backup-drive-wrapper.sh:

```
#!/bin/bash

# Find the absolute path of the script.
# See http://stackoverflow.com/a/246128/659910.
DIR="$(cd "$(dirname "${BASH_SOURCE[0]}")" && pwd)"

# This script is called by `udev` and needs to terminate quickly so we complete
# the remainder of the work as a background process.
# `$1` contains the device name.
nohup "${DIR}/sync-backup-drive.sh" ${1} >/dev/null 2>&1 &
```



Our script should terminate quickly and not tie up the event loop. To that end, we use nohup to create a detached process that will live on after the wrapper terminates. If our script does not terminate, Ubuntu will fail to auto-mount the drive. Here’s sync-backup-drive.sh:


```
#!/bin/bash

DRIVE_NAME="${1}"
USERNAME="$(whoami)"
SOURCE_DIR="/home/${USERNAME}/Backup"

if [ -z "${DRIVE_NAME}" ]; then
    exit 1
fi

function notify() {
    su "${USERNAME}" -c "DISPLAY=:0.0 /usr/bin/notify-send '${1}'"
}

# Wait for Ubuntu to auto-mount the drive.
drive_dir=
while [ -z "${drive_dir}" ]; do
    sleep 2
    drive_dir="$(grep "${DRIVE_NAME}" /etc/mtab | awk '{print $2}')"
done

notify "Syncing ${drive_dir}"
HOME=/root unison "${SOURCE_DIR}" "${drive_dir}" -batch -prefer newer -times=true -perms 0
notify "Finished syncing ${drive_dir}"
```

It simply polls /etc/mtab to detect when Ubuntu has finished mounting the drive. Unison then takes care of updating the files on the drive. Unison handles 2-way syncing more intelligently than rsync. In Ubuntu, it’s available by default via $ sudo apt-get install unison.

We’ve used the -batch flag to make Unison run without interruption. The -prefer newer and -times=true flags ensures Unison deals with conflicts by keeping the file with more recent modification time.

We’re also using notify-send for system-level notifications about when syncing begins and ends. It takes a bit of messing around to get them to display when called from udev so we’ve hidden that complexity in the notify function.

Bonus: Before unplugging your drive, you will want to unmount and spin it down:

```
#!/bin/bash

DRIVE_NAME=${1:-Backup}

if [ -z "${DRIVE_NAME}" ]; then
    echo 'No drive found.' 1>&2
    exit 1
fi

drive_dir="$(mount -l | grep "\[${DRIVE_NAME}\]" | awk '{print $1}')"
drive_dir_no_num=$(echo ${drive_dir} | sed -E 's,/dev/([a-z]*).*,/dev/\1,g')

# Unmounts and spins down an external drive.
udisks --unmount ${drive_dir}
udisks --detach ${drive_dir_no_num}
```

The script will assume your drive is named “Backup” but you can pass in any name as a command-line argument. I recommend putting the script in /usr/local/bin. Assuming that directory is part of your $PATH environment variable, you can now call the script like a regular shell command:
```
$ unmount.sh drive-name
```


http://mhluska.com/blog/2013/05/26/automatically-syncing-a-usb-drive-on-linux/















outra forma:

1
down vote
accepted
Ok, it's been a long time, but I'll still answer my question with the best option I found as of now. To summarize: create a udev rule, associated with some scripts (that will create/remove directories and un/mount removable devices), and attached to udev device event type=partition.

1 - Creating add / remove scripts

Save following script storage-automount.sh to /lib/udev/ and make it executable (sudo chmod a+x /lib/udev/storage-automount.sh):

#!/bin/sh

# set the mountpoint name according to partition or device name
mount_point=$ID_FS_LABEL
if [ -z $mount_point ]; then
    mount_point=${DEVNAME##*/}
fi

# if a plugdev group exist, retrieve it's gid set & it as owner of mountpoint
plugdev_gid="$(grep plugdev /etc/group|cut -f3 -d:)"
if [ -z $plugdev_gid ]; then
    gid=''
else
    chown root:plugdev $mount_point
    gid=",gid=$plugdev_gid"
fi

# create the mountpoint directory in /media/ (if not empty)
if [ -n $mount_point ]; then
    mkdir -p /media/$mount_point
    # other options (breaks POSIX): noatime,nodiratime,nosuid,nodev
    mount -t $ID_FS_TYPE \
      -o rw,flush,user,uid=0$gid,umask=002,dmask=002,fmask=002 \
      $DEVNAME /media/$mount_point
fi
Save following script storage-autounmount.sh to /lib/udev/ and make it executable (sudo chmod a+x /lib/udev/storage-autounmount.sh):

#!/bin/sh

# set the mountpoint name according to partition or device name
mount_point=$ID_FS_LABEL
if [ -z $mount_point ]; then
    mount_point=${DEVNAME##*/}
fi

# remove the mountpoint directory from /media/ (if not empty)
if [ -n $mount_point ]; then
    umount -l /media/$mount_point
    rm -R /media/$mount_point
fi
2 - Creating the udev rule to attach those scripts to events

And finally, add a udev rule in /etc/udev/rules.d/, for instance 85-storage-automount.rules:

ENV{DEVTYPE}=="partition", RUN+="/lib/udev/storage-automount.sh", ENV{REMOVE_CMD}="/lib/udev/storage-autounmount.sh"
and make it have the same permissions as the other rules in that dir/folder

Now, when you plug a storage device in, a directory will be created in /media/ according to the partition name (I don't remember but I think it's working with NTFS partition as well) and your partition will be mounted into it. It's R/W for users if you have a plugdev group on your system. Also, the devices are mounted in synchronous mode in order to limit the risks of data loss in case of hot unplugging.

When the device is removed, it's unmounted and the directory is removed from /media

Also, the tool to monitor the udev events is udevadm monitor, with options like --env or --property:

$ udevadm monitor --env
This is tested and working fine on both debian and arch, but probably work on all distributions that rely on udev.
http://unix.stackexchange.com/questions/44454/how-to-mount-removable-media-in-media-label-automatically-when-inserted-with

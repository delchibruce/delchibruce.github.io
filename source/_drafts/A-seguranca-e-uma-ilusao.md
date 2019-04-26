---
title: A segurança é uma ilusão
date: 2016-09-02 19:18:37
categories:
tags:
thumbnail:
---

Signing files

Signing files is similar to encrypting them, but by default the signature will use your private key. The recipient of a file will use your public key to verify the signature. By default, the resulting file will be a binary file. If you use the --armor option, it will produce an ASCII file with the .asc extension.

To sign a file with GPG, run the following command.

$ gpg2 --sign plain.txt

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 37BEB021, created 2015-11-15

Enter passphrase:
You can also sign it using ASCII.

$ gpg2 --armor --sign plain.txt

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 37BEB021, created 2015-11-15

Enter passphrase:
Remember that despite the fact that both of the files have been encrypted, anyone with your public key will be able to decrypt them and verify the signature.

Alternative ways to sign

There are two other options for signing files. If you want to leave the original contents unencrypted, you can use either --clearsign or --detach-sign. The --clearsign option will add a signature in ASCII directly to the file. The --clearsign option will only work as expected with plain text documents. If you want to leave a graphic or LibreOffice documents unencrypted but signed, you should use the --detach-sign option. This will create an additional file as a detached signature to be sent with the file.

You can use the --clearsign flag as follows.

$ gpg2 --clearsign plain.txt

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 37BEB021, created 2015-11-15

Enter passphrase:
The resulting file can be opened and read with an ASCII signature added to the file.

Using GPG: GPG --clearsign example

To use the --detach-sign option, run this command.

$ gpg2 --detach-sign test.odt

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 37BEB021, created 2015-11-15

Enter passphrase:
This results in the original file being unmodified and an additional file named test.odt.sig that can then be verified using the following command.

$ gpg2 test.odt.sig
gpg: assuming signed data in `test.odt'
gpg: Signature made Sun 07 Feb 2016 10:37:49 PM EST using RSA key ID 37BEB021
gpg: Good signature from "charles profitt <fedora@cprofitt.nrl>"
The signature can only be verified if the original file is available to the user. If the original file is in the same directory it will be used automatically. If the file is not then the you will be prompted to provide the path to the original file.

Regardless of the type of signature used, if the file was modified, GPG will report a bad signature.

$ gpg2 test.odt.sig
gpg: assuming signed data in `test.odt'
gpg: Signature made Sun 07 Feb 2016 10:37:49 PM EST using RSA key ID 37BEB021
gpg: BAD signature from "charles profitt <fedora@cprofitt.nrl>"
You can also encrypt and sign a file at the same time by combining the commands as follows:

$ gpg2 --armor --recipient fedora@cprofitt.nrl --encrypt --sign plain.txt

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 37BEB021, created 2015-11-15

Enter passphrase:
When you decrypt the resulting file, you will get additional output verifying the signature was good or bad.

$ gpg2 plain.txt.asc

You need a passphrase to unlock the secret key for
user: "charles profitt <fedora@cprofitt.nrl>"
4096-bit RSA key, ID 853D07A8, created 2015-11-15 (main key ID 37BEB021)

gpg: encrypted with 4096-bit RSA key, ID 853D07A8, created 2015-11-15
      "charles profitt <fedora@cprofitt.nrl>"
gpg: Signature made Sun 07 Feb 2016 11:12:26 PM EST using RSA key ID 37BEB021
gpg: Good signature from "charles profitt <fedora@cprofitt.nrl>"












Bitcoin Core release signature verification

Posted on February 25, 2015
Before you can verify the Bitcoin Core release signature you need to perform two steps:

Obtain the release you want to use and the corresponding signature file
Obtain the key the release was signed with
Obtain the release

Download the official tarball release:

wget https://bitcoin.org/bin/bitcoin-core-0.10.0/bitcoin-0.10.0.tar.gz

You can find the latest source code release tarball here.

Download the file containing the signature over the list of hashes calculated for all files included into the release:

wget https://bitcoin.org/bin/bitcoin-core-0.10.0/SHA256SUMS.asc

Obtain the key

Every release is signed by one of the core developers. You can find their public keys here. The release in this example was signed by Wladimir J. van der Laan.

Download and import Wladimir’s public key:

wget https://bitcoin.org/laanwj.asc
gpg --import laanwj.asc

Once imported it can be used to verify the signature.

Signature verification

Having these two steps out of your way you are now ready to verify the tarball. The validation is again a two step process.

1. Verify cryptographic signature of the SHA256SUMS.asc file containing the set of hashes.

gpg --verify SHA256SUMS.asc

If the signature is valid the output should say “Good signature”:

gpg: Signature made Mon 16 Feb 2015 08:38:00 AM CET using RSA key ID 2346C9A6
gpg: Good signature from "Wladimir J. van der Laan <laanwj@gmail.com>"
gpg: WARNING: This key is not certified with a trusted signature!
gpg: There is no indication that the signature belongs to the owner.
Primary key fingerprint: 71A3 B167 3540 5025 D447 E8F2 7481 0B01 2346 C9A6

Open the SHA256SUMS.asc  file and locate the name of the tarball file you downloaded and corresponding hash value. You are now sure what the correct hash value of your bitcoin-0.10.0.tar.gz tarball is:

a516cf6d9f58a117607148405334b35d3178df1ba1c59229609d2bcd08d30624

2. Calculate the SHA-256 hash over downloaded tarball:

sha256sum bitcoin-0.10.0.tar.gz

Compare the result with the validated hash value listed in the verified SHA256SUMS.asc file. If the hash values are the same you are sure the tarball you have downloaded was not tampered with and it was signed by one of the Bitcoin core developers.

This entry was posted in Bitcoin, GPG, Tutorial. Bookmark the permalink.

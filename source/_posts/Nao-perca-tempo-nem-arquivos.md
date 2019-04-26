---
title: 'Não perca tempo, nem arquivos'
date: 2016-09-18 06:47:25
categories:
- Linux
- Backup
tags:
- aplicativos
- backup
- rsync
- sincronizar
- pendrive
thumbnail: /images/hd_frito.png
---
![](/images/hd_frito.png)

Vírus, malwares e, principalmente, falhas de hardware podem fazer com que percamos nossos dados mais importantes.  Eu, particularmente, uso a nuvem pública para fazer backup de documentos e dados não sensíveis e uma nuvem particular para as coisas mais importantes.

Mas, mesmo assim, ainda faço backup em discos rígidos e em pendrives. Mas fazer backup manualmente é muito chato!

Esse aqui é o método que uso para automatizar meus backups em disco.

<!-- more -->

## Para que?
Ano passado meu computador morreu. De vez mesmo. Mas eu nem me estressei, porque tudo de importante estava seguramente guardado. Instalei meu OpenSUSE na nova máquina, adicionei os repositórios e atualizei tudo em menos de 30 minutos. Depois conectei minhas contas do dropbox e mega e atulizei meus arquivos gerais. E, por fim, sincronizei meus arquivos sensíveis com meus discos externos e pendrives.

Tudo não demorou nem 1 hora e eu estava com a nova máquina devidamente atualizada e com todos os meus arquivos disponíveis.

Mas se eu não tivesse esses backups eu estaria doido da vida. Perdido!

E isso é especialmente importante para concurseiros que estão dedicando seu tempo (e suado dinheirinho) para estudar e passar em um concurso público. Então, se você está usando uma das [ferramentas de anotação](/2016/09/01/Os-melhores-apps-para-anotar/index.html) que sugeri, ou tem arquivos importantes para seus estudos guardados na sua máquina, não deixe de se proteger.

Não importa se usa windows, Mac ou Linux, fazer backup é importante e pode te salvar de muita dor de cabeça.


## A ferramenta
Existem diversas ferramentas que fazem o que vou sugerir agora. Eu uso sempre o [Synkron]() e o [rsync](). A solução que darei a seguir usa o rsync.

O rsync foii criado em 1996 e serve tanto como aplicativo de sincronia como de transferência de arquivos. Uma das diversas vantagens desse programa é que usa um algoritmo de compressão que reduz o tamanho dos dados a serem transferidos, servindo, portanto, para transferências locais como remotas.

O rsync é uma aplicação padrão nas distribuições linux, mas pode ser encontrada para [windows](https://www.itefix.net/cwrsync) e [mac (com interface gráfica)](http://arrsync.sourceforge.net).

A ideia que apresento abaixo é muito simples: você conecta o pendrive ou HD externo e, automaticamente, fará o backup das informações desejadas. É só isso mesmo: encaixa o USB e espera o sistema fazer o backup pra você. Depois é só guardar seu HD externo ou pendrive, com todos os arquivos atualizados.

Para saber se você tem o rsync instalado corretamente, vá ao terminal e digite `rsync --version`. Se sair alguma coisa assim, tá tudo certo:
```
bruce@LAB:~> rsync --version
rsync  version 3.1.2  protocol version 31
Copyright (C) 1996-2015 by Andrew Tridgell, Wayne Davison, and others.
Web site: http://rsync.samba.org/
Capabilities:
    64-bit files, 64-bit inums, 64-bit timestamps, 64-bit long ints,
    socketpairs, hardlinks, symlinks, IPv6, batchfiles, inplace,
    append, ACLs, xattrs, iconv, symtimes, prealloc, SLP

rsync comes with ABSOLUTELY NO WARRANTY.  This is free software, and you
are welcome to redistribute it under certain conditions.  See the GNU
General Public Licence for details.
```

Se você está usando windows ou mac, pule para a seção [`O backup em si`](#markdown-o-backup-em-si)

## Montando automaticamente os HDs e pendrives

Vamos começar descobrindo o `idVendor`e o `idProduct`do seu HD ou pendrive. É bem fácil, só tendo que digitar no terminal `lsusb`.

O resultado trará algumas linhas, mas a que você quer tem o nome do seu HD ou pendrive. Olha o meu:
```
Bus 001 Device 002: ID 8087:8000 Intel Corp.
Bus 001 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
Bus 003 Device 001: ID 1d6b:0003 Linux Foundation 3.0 root hub
Bus 002 Device 005: ID 0bda:b728 Realtek Semiconductor Corp.
Bus 002 Device 004: ID 5986:055e Acer, Inc
Bus 002 Device 003: ID 0bda:0129 Realtek Semiconductor Corp. RTS5129 Card Reader Controller
Bus 002 Device 039: ID 13fe:1f00 Kingston Technology Company Inc. DataTraveler 2.0 4GB Flash Drive / Patriot Xporter 32GB (PEF32GUSB) Flash Drive
Bus 002 Device 002: ID 062a:4101 Creative Labs
Bus 002 Device 001: ID 1d6b:0002 Linux Foundation 2.0 root hub
```

Tá vendo a linha que tem `Kingston`? Essa é linha do pendrive. O que vem logo depois do `ID` é o que você quer.

Meu `idVendor` é "13fe" e o `idProduct` é "1f00". Você vai precisar disso na última linha do código abaixo.


Vamos criar uma regra para montar automaticamente os HDs externos e os pendrives. Crie essa regra em /etc/udev/rules.d/11-media-by-label-auto-mount.rules:
```
KERNEL!="sd[a-z][0-9]", GOTO="media_by_label_auto_mount_end"  
# Import FS infos  
IMPORT{program}="/sbin/blkid -o udev -p %N"  
# Get a label if present, otherwise specify one  
ENV{ID_FS_LABEL}!="", ENV{dir_name}="%E{ID_FS_LABEL}"  
ENV{ID_FS_LABEL}=="", ENV{dir_name}="usbhd-%k"  
# Global mount options  
ACTION=="add", ENV{mount_options}="relatime"  
# Filesystem-specific mount options  
ACTION=="add", ENV{ID_FS_TYPE}=="vfat|ntfs", ENV{mount_options}="$env{mount_options},utf8,gid=100,umask=002"  
# Mount the device  
ACTION=="add", RUN+="/bin/mkdir -p /media/%E{dir_name}", RUN+="/bin/mount -o $env{mount_options} /dev/%k /media/%E{dir_name}"  
# Clean up after removal  
ACTION=="remove", ENV{dir_name}!="", RUN+="/bin/umount -l /media/%E{dir_name}", RUN+="/bin/rmdir /media/%E{dir_name}"  
# Exit  
LABEL="media_by_label_auto_mount_end"
# começa o bkp
KERNEL=="sd?1", ACTION=="add", ATTRS{idVendor}=="13fe", ATTRS{idProduct}=="1f00", RUN+="/home/bruce/Downloads/start_bkp.sh"
```
E depois, no terminal, recarregue o udev com:
```
sudo udevadm control --reload-rules
```
Pronto! Toda vez que os HD externos e pendrives forem inseridos eles serão montados automaticamente, de acordo com o Label que você deu, na pasta `/media/`. O label do pendrive que vou usar é black (é daqueles kingstons coloridos e chamo cada um deles pela cor).

## O backup em si

Eu acho melhor criar um backup de sincronia. Isso significa que a pasta local ficará igualzinha no pendrive/HD. Mais isso vai do gosto do freguês. Se quiser fazer diferente, comenta lá em baixo que eu tento ajudar.

Agora vamos criar dois arquivos. O primeiro é um script bem simples que serve para chamar o script de backup. Ele é necessário para não entupir a fila do udev.

O primeiro eu chamei de `start_bkp.sh`. Criei em ~/Downloads para testar, mas vou mudar para `~/bin/scripts` para ficar direitinho.

```bash
#!/bin/bash
/home/bruce/Downloads/bkp_sync.sh & exit
```
Agora o `bkp_sync.sh` que é o script de backup em si:

```bash
#!/bin/sh
# esperar 5s para terminar de montar
sleep 5
exitcode=1
# checar se pendrive/HD está montado
if test -e '/media/black';then
exitcode=0
#sincronizar 2 pastas
rsync -rtvuc --delete /home/bruce/Downloads/sync/ /media/black/
fi
# se não estiver montado, termina com exitcode=1
exit $exitcode
```

## Notificações nativas
Se você quiser que apareça uma notificação na sua barra de tarefas é só colocar esse código aqui:

```bash
# aparece notificação nativa na DE
sleep 5
if [ $? -ne 1 ]
  then
    notify-send -t 0 "Backup terminado $(date)"
  else
    notify-send -t 0 "Deu pau no backup $(date)"
fi
```

## Resumo da ópera
Só para resumir o que foi feito:
- instalação do rsync (padrão nas distribuições linux);
- criação de regra udev para automontagem das mídias removíveis em `/etc/udev/rules.d/`;
- reload das regras do udev, usando `sudo udevadm control --reload-rules`;
- criação de script bash de relay. Chamei o meu de `start_bkp.sh`;
- criação do script de backup. O meu é o `bkp_sync.sh`;
- adicione, opcionalmente, o fragmento de notificação.

**E isso é tudo!**

É bem fácil adaptar esses scripts às suas necessidades. Atente para os esses fatos:
- caminho da pasta externa: no meu caso `/media/black/`
- caminho da pasta de origem: aqui é a `/home/bruce/Downloads/sync/`
É só procurar esses caminhos nos meus scripts e mudar pelos seus.

<hr>
Muito bem!
A partir de agira seus backups serão feitos rapidamente e sem dor de cabeça. **É só plugar o HD externo ou o pendrive e esperar a término da transferência dos arquivos**.

Abraços produtivos!

delchi bruce

<hr>
Fontes:
- [Auto-mounting USB storage with udev](https://www.axllent.org/docs/view/auto-mounting-usb-storage/)
- [How to auto-sync with a plugged-in USB mass storage device?](http://unix.stackexchange.com/questions/66749/how-to-auto-sync-with-a-plugged-in-usb-mass-storage-device)
- [Synchronizing folders with rsync](http://www.jveweb.net/en/archives/2010/11/synchronizing-folders-with-rsync.html)

---
title: Como instalar o spotify no opensuse
date: 2017-02-01 6:08:03
categories:
-   Aplicativos
-   Linux
tags:
- spotify
- linux
- música
thumbnail: /images/spotify_browse.png
---
![](/images/spotify_browse.png)
O Spotify é sinistro!

É possível achar tudo quanto é tipo de música e montar playlist pra tudo: estudar, programar, cozinhar, para festas e jantares e tudo mais que você quiser.

**Só que o spotify não liga pra galera do Linux. Nem. um. pouco.**

<!-- more -->

Ainda bem que a comunidade é bem ativa e o cornguo fez um script que arruma tudo e instala pra você só se preocupar em ouvir música (e trabalhar/estudar)!

São só dois passos: pegar o script e rodá-lo. Comece clonando o repositório git:
```
git clone https://github.com/cornguo/opensuse-spotify-installer.git
```

Vai ser criado um diretório chamado `opensuse-spotify-installer`.

Agora é só entra no diretório, mudar a permissão do script e rodar o bixo (precisa da senha de super usuário):

```
cd opensuse-spotify-installer
sudo chmod +x install-spotify.sh
./install-spotify.sh
```

O script vai baixar tudo, fazer as atualizações de bibliotecas, pegar o deb e transformar em rpm e instalar o spotify. Ele serve para atualizar o spotify também, viu?

Se você não achar o spotify no seu laucher, ele está aqui: `/usr/bin/spotify`.

Valeu, cornguo!!

<div style="padding: 5px 20px; margin: 5px; background: #F0F3F1;"><img src="/images/new_eu_round_pad.png" style="float:left;width:115px;height:115px;">**Delchi Bruce** estudou Relações internacionais e é professor de cursinho desde 2008. É apaixonado por formas de melhorar a produtividade e ser mais eficiente. É gestor de conteúdo do [Mapa da Prova](http://mapadaprova.com.br), ferramenta de estudo para concurseiros. Usa linux [(openSUSE)](http://opensuse.org) desde 1999 e faz trade de [bitcoin](http://bitcoin.org) todo dia.
Você pode entrar em contato [por aqui](/contato/).</div>

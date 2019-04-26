---
title: Como instalar o f.lux no OpenSUSE Leap 42.1
date: 2016-08-04 20:01:51
categories:
 - Linux
tags:
 - produtividade
 - opensuse
 - luz
 - howto
thumbnail: /images/luz_azul.png
---

![](/images/luz_azul.png)
Essa semana eu falei sobre como é importante dormir bem e acordar bem disposto para render bem nos estudos.

Uma das dicas que os especialistas sempre levantam é reduzir a quantidade de luz azul a que nos submetemos antes de dormir. Essa luz está nos televisores, monitores e telefones e prejudica o sono.

<!-- more -->

Isso porque a luz azul inibe a produção de malatonina, que é um hormônio **cuja principal função é regular o sono**. A luz azul prejudica a produção desse hormônio em até 50%. Então, se você não consegue ficar longe do computador, seja por causa dos estudos, de trabalho, ou diversão, sugiro usar o [f.lux](https://justgetflux.com).

Esse aplicativo é interessantíssimo! Ele altera as cores do monitor, ajustando a quantidade de luz azul a partir de uma certa hora. Então, de noite a tela fica com cores mais quentes. Não ache que não dá pra ver a diferença, porque ela é perceptível.

Essa diferença de cores vai diminuir a interferência do computador na sua produção de melatonina e te ajudar a dormir!

É bem fácil instalar esse programa no windows, no mac e no ubuntu, já que o pessoal disponibiliza o .deb no site. Mas, pro OpenSUSE nós fazemos assim:

## Instalação

#### 1º Baixe o pacote
VocÊ precisará baixar o pacote xflux diretamente do [site deles](https://justgetflux.com/linux.html).

Lembre-se que você vai querer o **xflux daemon 64 bits**. O arquivo que baixei foi o xflux64.tgz.


#### 2º Movendo para o local corretamento
Vamos descompactar o arquivo baixado;
Vá para a pasta em que está o arquivo baixado e rode:

```
tar zxvf xflux64.tgz
```
Um arquivo chamado xflux vai ser criado. Agora vamos movê-lo.

```
sudo mv xflux /usr/bin/
```
E vamos alterar as permissões do novo arquivo. Essa permissão é, também, conhecida como drwxr-xr-x.
```
sudo chmod 755 /usr/bin/xflux
```
## Ligando o f.lux
<div style="float: center">
<img src="/images/logos/f.lux.png" width="500" margin-right:="30px">
</div>
Viu como foi fácil instalar? Só descompactar e mover, com um pequeno ajuste de permissões de acesso.

Para iniciar o programa não é só clicar no arquivo que baixamos. E por que não? Por precisarmos inserirmos as coordenadas geográficas (latitude e longitude). Assim o f.lux vai saber a hora de começar a mudar a temperatura das cores.


#### Achando suas coordenadas
Se você não sabe suas coordenadas, pode usar qualquer site de gps, mapas etc. Mas é só entrar numa [página específica do f.lux](https://justgetflux.com/map.html) que você conseguirá ver sua latitude e longitude. O browser vai pedir pra saber sua localização.

#### Iniciando o programa
No terminal:
```
xflux -l -15, -g -47
```
O -l vem antes da latitude e o -g antes da longitude. Eu coloquei os dados de Brasília, para ilustrar. Você precisa colocar as suas coordenadas.

Essa vai ser a mensagem que virá. Isso significa que **deu certo e a aplicação tá funcionando direitinho**.
```
--------
Welcome to xflux (f.lux for X)
This will only work if you're running X on console.
Found 1 screen.
Your location (lat, long) is -15.0, -47.0
Your night-time color temperature is 3400
It's night time. Your screen is changing now.
Going to background: 'kill 20548' to turn off.
```

#### Desligando o app
Se você precisar desligar o app, é só terminar o processo. Na mensagem acima o processo é o 20548.

## Fazendo o f.lux ligar iniciar sozinho

Tem gente que usa KDE, Gnome, XFC, etc etc etc. Então, você pode usar a ferramenta gráfica de configuração da sua DE para incluir o 'xflux -l -15, -g -47' (com as suas coordenadas) no inicializar ou pode fazer dessa forma aí embaixo, que serve para qualquer DE, ou mesmo OS:

Vamos criar um arquivo .desktop na pasta de inicialização do usuário, que fica em ~/.config/autostart/.

```
cd ~/.config/autostart/
cat > xflux.desktop
```

Ao digitar essa última linha aí em cima você **não voltará para o prompt**, mas poderá digitar o texto do arquivo recém criado.

Ó ele aqui:

```
[Desktop Entry]
Exec=xflux -l -15.8332, -48.0313\n
```
Ao reiniciar o computador, o f.lux já vai ser iniciado e de noite você verá a diminuição das luzes azuis no monitor.

Espero que isso te ajude a dormir melhor!

sucesso
delchi bruce

---
title: Como gerar paper wallets pelo terminal
date: 2016-09-27 19:12:20
categories: Bitcoin
tags:
- carteiras
- wallet
- paper wallets
- tails
- bitcoin core
thumbnail: /images/btc_papel_qr.png
---
![](/images/btc_papel_qr.png)
Hoje um amigo de longa data pediu ajuda para "entrar no mundo bitcoin". Falei que era pra já!

Expliquei o funcionamento do protocolo, da moeda, das altcoins e ajudei na configuração dos cães de guarda. Ele fez a primeira compra. Depois disso tudo, pediu uma carteira offline, de papel.

Então fizemos umas _paper wallets_ em modo _offline_. Tudo muito seguro!

<!-- more -->

## Considerações iniciais
Antes de começarmos, de fato, preciso deixar claro algumas coisas:
- usar geradores de endereços que sejam online pode ser uma grande furada. Prefira geral chaves localmente;
- é melhor usar ambientes completamente desconectados da internet. Prefira linux;
- mas não é necessário usar a distribuição que sugiro. Pode-se usar o OpenSUSE, ou outra de sua preferência;
- não gere chaves privadas no windows. Vírus e outros malwares podem se aproveitar de você;
- antivírus não são 100% seguros ou confiáveis;
- não deixe de verificar a integridade do programa baixado. Verifique a assinatura e o sha256 dos arquivos baixados;
- não envie suas chaves privadas por email;
- não bata fotos da sua chave ou salve-as no telefone;
- se for mesmo salvar em algum lugar, prefira um pendrive ou memory card **criptografado**.

## Baixando a carteira Bitcoin
Vamos começar baixando a carteira Bitcoin do site do [projeto](http://bitcoin.org). Nesse caso específico usamos o bitcoin-core para linux x86_64. Ajuste de acordo com o que você usar. Em seguida, baixamos o checksum em sha256, também disponível no site do projeto.

Esse arquivo `SHA256SUMS.asc` serve para dar mais confiança de que o arquivo da carteira baixado (no meu caso `bitcoin-0.13.0-x86_64-linux-gnu.tar.gz`) é realmente o que os desenvolvedores lançaram e não foi interceptado por _agentes do lado negro da força_. Existem mais formas de confirmar essa viabilidade, mas vamos usar essa.

Abra o arquivo .asc usando um editor de textos e você terá, entre outras coisas, isso aqui:
```
f94123e37530f9de25988ff93e5568a93aa5146f689e63fb0ec1f962cf0bbfcd  bitcoin-0.13.0-aarch64-linux-gnu.tar.gz
7c657ec6f6a5dbb93b9394da510d5dff8dd461df8b80a9410f994bc53c876303  bitcoin-0.13.0-arm-linux-gnueabihf.tar.gz
d6da2801dd9d92183beea16d0f57edcea85fc749cdc2abec543096c8635ad244  bitcoin-0.13.0-i686-pc-linux-gnu.tar.gz
2f67ac67b935368e06f2f3b83f0173be641eef799e45d0a267efc0b9802ca8d2  bitcoin-0.13.0-osx64.tar.gz
e7fed095f1fb833d167697c19527d735e43ab2688564887b80b76c3c349f85b0  bitcoin-0.13.0-osx.dmg
0c7d7049689bb17f4256f1e5ec20777f42acef61814d434b38e6c17091161cda  bitcoin-0.13.0.tar.gz
213e6626ad1f7a0c7a0ae2216edd9c8f7b9617c84287c17c15290feca0b8f13b  bitcoin-0.13.0-win32-setup.exe
5c5bd6d31e4f764e33f2f3034e97e34789c3066a62319ae8d6a6011251187f7c  bitcoin-0.13.0-win32.zip
c94f351fd5266e07d2132d45dd831d87d0e7fdb673d5a0ba48638e2f9f8339fc  bitcoin-0.13.0-win64-setup.exe
54606c9a4fd32b826ceab4da9335d7a34a380859fa9495bf35a9e9c0dd9b6298  bitcoin-0.13.0-win64.zip
bcc1e42d61f88621301bbb00512376287f9df4568255f8b98bc10547dced96c8  bitcoin-0.13.0-x86_64-linux-gnu.tar.gz
```
Aí, no terminal, você entra com o comando para verificar se a soma sha256 do seu arquivo é a mesma do arquivo originalmente disponibilizado pelos desenvolvedores.
```
bruce@LAB:~/Downloads> sha256sum bitcoin-0.13.0-x86_64-linux-gnu.tar.gz
bcc1e42d61f88621301bbb00512376287f9df4568255f8b98bc10547dced96c8  bitcoin-0.13.0-x86_64-linux-gnu.tar.gz
```
Você pode ver que o resultado da checagem é `bcc1e42d61f88621301bbb00512376287f9df4568255f8b98bc10547dced96c8`, igual ao número disponível no arquivo `SHA256SUMS.asc`. Isso quer dizer que tá tudo bonito e podemos continuar!

Copie o arquivo baixado para um pendrive novo ou higienizado. Separe.

## Rode o T.A.I.L.S
O segundo passo foi pegar o ISO da distribuição Tails e fazer um pendrive de boot. Esse é outro pendrive, viu?]

Entre no site do [projeto Tails](https://tails.boum.org/index.en.html) e pegue o ISO da última versão (no meu caso é a 2.6). Você vai precisar:
- baixar o ISO da Tails. Um pouco mais de 1.1 GB;
- baixar a assinatura digital (um arquivo .sig);
- baixar a chave de assinatura da Tails.

#### Verifique a chave de assinatura
Muito fácil e rápido. Só um comando no terminal:
```
bruce@LAB:~/Downloads> gpg --import tails-signing.key
```
O terminal vai dizer se deu certo ou não. No meu caso, que já tinha feito isso, vem essa resposta:
```
gpg: key 58ACD84F: "Tails developers (offline long-term identity key) <tails@boum.org>" not changed
gpg: Número total processado: 1
gpg:              não modificados: 1
```

#### Verifique a assinatura do arquivo ISO
Mais fácil ainda:
```
gpg --keyid-format 0xlong --verify tails-i386-2.6.iso.sig tails-i386-2.6.iso
```
A resposta é grandinha, mas o que precisa vir é isso aqui: `gpg: Good signature from "Tails developers (offline long-term identity key) <tails@boum.org>" [full]`. Se veio, quer dizer que a imagem baixada está nos conformes e você pode continuar.

#### Gravando a imagem em um pendrive
Desmonte seu pendrive:
```
umount /dev/sdX
```
O meu é sdc.... então vou continuar com o sdc. Você precisa saber qual é a identificação do seu pendrive. Para tanto, use, no terminal: `ls -l /dev/disk/by-id/*usb*`. No meu a resposta que veio foi `> ../../sdc`.

Aí é só mandar salvar no pendrive!
```
dd_rescue /~/Downloads/tails-i386-2.6.iso /dev/sdc
```
ATENÇÃO: tenha certeza que usou a letra do drive certo!!! Se não pode detonar sua distribuição! **Verificar esse dado é responsabilidade sua!**

Antes de continuar é preciso deixar uma coisa muito clara: esse pendrive vai permitir usar uma distribuição linux feita para anonimato e segurança. Ao usá-la você **não deve conectá-la à internet**!. Então, antes de continuar, desconecte o cabo de rede, caso exista e lembre-se de não conectar ao wifi. Dê boot pelo pendrive e divirta-se com sua nova distribuição Tails!

## Usando o Bitcoin Core
Agora que você está usando uma versão isolada (air-gapped) de uma boa distribuição linux, sem contato nenhum com a internet, podemos continuar e gerar nossas carteiras offline de Bitcoin.

Pegue o segundo pendrive (aquele que você copiou o bitcoin-core) e conecte no PC. Extraia o arquivo usando `tar -zxvf bitcoin-0.13.0-x86_64-linux-gnu.tar.gz`.

Entre na pasta criada, inicie a carteira e gere quantos endereços quiser. Os endereços são necessários para depositar bitcoins.
```
start ./bitcoind
./bitcoin-cli getnewaddress
```
Agora dê um dump da chave privada. A chave privada é necessária para usar os bitcoins disponíveis no endereço:
```
./bitcoin-cli dumpprivatekey endereço_gerado_no_passo_anterior
```
Substitua `endereço_gerado_no_passo_anterior` pelo endreço gerado no passo anterior. Essas chaves privadas vem no formato de importação da carteira, codificado em Base58. Com essa chave você consegue importar e usar seus bitcoins.

## Criando o QR code dos endereços
Agora vamos criar os QR codes para imprimir e guardar bem guardado. Comece instalando o `qrencode`:

E no Tails é diferente do OpenSUSE. Faça isso aqui:

```
apt-get update
apt-get install qrencode
```
Eu uso o formato `chave_públlica.png` para o arquivo que contem a chave privada de cada endereço. Assim quando você for procurar a chave privada que quer usar é só ir atrás do arquivo com o nome do endereço (chave pública) de bitcoin que contem suas moedas.
Gere as imagens de todos os endereços criados:
```
qrencode -o chave_publica.png 'chave_privada'
display chave_publica.png
```

#### Imprimir a chave privada
Para imprimir a imagem, digite:
```
lpr chave_publica.png
```
Faça isso para cada arquivo .png criado.

#### Limpar o histórico do terminal
Terminou? Limpe o histórico, se for continuar Usando o tails, ou desligue tudo.
```
history -c
history -w
```
ATENÇÃO: algumas impressoras possuem memória interna. Veja no manual da sua impressora se ela tem e como limpá-la.


Pronto! Tudo criado de forma segura.
Bora depositar uns bitcoins nessas carteiras?

Abraços libertários!

delchi bruce

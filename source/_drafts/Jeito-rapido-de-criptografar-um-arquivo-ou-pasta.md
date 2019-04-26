---
title: Jeito rápido de criptografar um arquivo ou pasta
date: 2017-01-31 00:40:13
categories:
tags:
thumbnail: /images/safe_deposit_boxes_pintado.png
---
![](/images/safe_deposit_boxes_pintado.png)

Toda semana ficamos sabendo de hackers que invadem computadores alheios ou arquivos que são acessados ilegalmente quando você leva seu computador pra consertar.

Isso me deixa tenso, principalmente se falarmos de arquivos importantes e com informações pessoais sensíveis.

Esse é um dos motivos pelo qual uso criptografia nos meus arquivos mais importantes.

Vou mostrar como faço e como você pode fazer. Tem até um script facinho pra ajudar!

<!-- more -->

## Usando o GPG
Vamos usar a chave importada para criptografar um arquivo de texto? É só fazer isso:
```
gpg --recipient bruce --armor --encrypt meu_arquivo_secreto.txt
```
O arquivo `meu_arquivo_secreto.txt` só poderá ser lido por quem tem a chave privada relacionada à chave pública usada. No caso, eu mesmo.
Para descriptografá-lo, use o comando abaixo:
```
gpg --output meu_arquivo_secreto.txt --decrypt meu_arquivo_secreto.asc
```
Pronto! o arquivo `meu_arquivo_secreto.txt` está prontinho para ser lido!

## Usando o script
```bash
#!/bin/bash
#########################################################
## script simples de criptografia de arquivo ou pasta  ##
##   Criado por Delchi Bruce      ##        8ruc3      ##
#########################################################

# sai do script se alguma coisa der pau
set -e

# texto de introdução
echo -e "\e[32mEsse é um script que criptografa um arquivo ou pasta usando o GPG.\e[0m"
echo -e "Ele só funciona se estiver na mesma pasta do que será criptografado."
echo -e "\e[31mAtenção: o arquivo ou pasta original será apagado depois que a cópia criptografada for criada.\e[0m"
echo -e "Qual é o arquivo ou a pasta que você quer criptografar?"

# seletor de arquivo
read file;

# comando de criptografia
gpg -c $file

if [ $? -ne 0 ]; then
  echo -e "\e[31mDeu pau! Não funfou! ERRO!!\e[0m";
	 #do the needful / exit
fi;

# mais textos explicativos
echo -e "\e[32mCópia criptografada criada! =D\e[0m"
echo -e "\e[31mDeletando arquivo original\e[0m"

# comando de remoção do arquivo/pasta original
rm -rf $file

# texto final. Não valida, só mostra texto
echo -e "\e[1mPronto! Tudo seguro!\e[21m"

# manda a notificação pro desktop
notify-send 'Script de criptografia' 'Pronto! Tudo seguro!!'

```

![](/images/janela_de_senha.png)

![](/images/janela_de_senha2.png)


## Baixe o script
[script de criptografar arquivo/pasta](/arquivos/criptografador.sh)

<div style="padding: 5px 20px; margin: 5px; background: #F0F3F1;"><img src="/images/new_eu_round_pad.png" style="float:left;width:115px;height:115px;">**Delchi Bruce** estudou Relações internacionais e é professor de cursinho desde 2008. É apaixonado por formas de melhorar a produtividade e ser mais eficiente. É gestor de conteúdo do [Mapa da Prova](http://mapadaprova.com.br), ferramenta de estudo para concurseiros. Usa linux [(openSUSE)](http://opensuse.org) desde 1999 e faz trade de [bitcoin](http://bitcoin.org) todo dia.
Você pode entrar em contato [por aqui](/contato/).</div>

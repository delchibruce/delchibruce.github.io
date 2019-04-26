---
title: Usando criptografia no dia a dia
date: 2016-08-18 22:53:26
tags:
- criptografia
- gpg
- gpg4win
- gpgtools
- assimétrica
- segurança
categories:
  - Aplicativos  
thumbnail: /images/cadeados.png
---

![](/images/cadeados.png)

Usamos criptografia todos os dias e, muitas vezes, nem percebemos. Ao acessar o banco, sites com `https`, ou logar no seu email, assistir um DVD ou blu-ray, enviar e receber bitcoins, usar seu cartão de crédito, fazer compras pela internet etc, etc e muito mais!

O fato é que muitos usam a criptografia apenas passivamente. Mas, quando você descobre o que pode fazer com todas as ferramentas disponíveis por aí, vai fazer como eu e **usar criptografia no dia a dia**.
<!-- more -->

## O que é criptografia?
>Criptografia é a prática ou o conjunto de técnicas para comunicação segura.

Isso significa, de fato, o estudo e a aplicação de **protocolos de comunicação** que impedem que seus adversários possam acessar informações sensíveis.

Uma das mais antigas formas de criptografia é a chamada `Cifra de César`, usada durante o Império Romano pelo próprio César em comunicação com seus aliados. De lá para cá as coisas evoluíram muito e o advento da informática e da evolução na teoria dos números permitiu a criação de sistemas complexos e bem resistentes de protenção de dados.

A mais famosa dessas inovações é a chamada `Pretty Good Privacy` criada em 1991 e antecessora dos sistemas atuais. A PGP, como ficou conhecida, deu origem ao padrão criptográfico OpenPGP, que é usado pelos mais diversos sistemas, mudo afora.

Do OpenPGP surgiu o GNU Privacy Guard (GnuPG or GPG), que é a aplicação mais usada para criptografia. Ele tem o código aberto, então qualquer um pode ver como funciona e se faz o que se propõe.

Derivativos do GPG existem aos montes e alguns são muito bons. Sugiro esses aqui:  
- para linux, o super [GnuPG](https://gnupg.org/)
- para windows, use o [Gpg4win](http://www.gpg4win.org/)
- para Mac, use o [GPGTools](https://gpgtools.org)
- para Android, use o [openkeychain](https://www.openkeychain.org)

## Por que criptografar?

Existem três motivos fundamentais para usar criptografia:

O primeiro é a proteção da **informação**. Você não quer que outra pessoa (ou organização) acesse seus dados. As informações são suas e você não acha que quem não tem sua autorização para acessá-las deva fazê-lo. Então, para proteger o que é seu, você usa a criptografia.

O outro, parecido com o primeiro, é proteger a **comunicação**. Nesse sentido, você e outra pessoa não querem que bisbilhotem a conversa de vocês. Pois bem, o direito à privacidade pode ser reforçado pela criptografia.

Por fim, temos um motivo que parece mais simples mas que é, também, importantíssimo: assegura a **identidade** de alguém. Na internet de hoje é até fáceil se passar por outra pessoa. Pode-se criar emails e contas falsas e tentar fingir ser quem não se é. A criptografia dificulta isso.

## O par de chaves

Tanto o PGP quanto o GPG funcionam hibridamente. Isso quer dizer que você pode usar tanto chaves simétricas quanto assimétricas.

> A criptografia de chave simétricas é quando a mesma senha é usada para criptografas e descriptografar a informação. Na assimétrica, são necessárias chaves distintas.

No meu caso, em especial, uso sempre o sistema de chaves assimétricas. E recomendo que você faça o mesmo. Nesse sistema será gerado um **par de chaves**, sendo uma chave pública, que você poderá distribuir e será usada para criptografar a informação. Qualquer pessoa que tiver sua chave pública poderá lhe enviar informações criptografadas, mas só quem tiver a chave privada poderá descriptografá-la. A chave privada é usada para acessar as informações protegidas. Essa é só sua e ninguém deve ter acesso.

Para continuarmos vou partir do pressuposto que você instalou o GPG específico do seu sistema operacional e está usando _prompt_ de comando ou terminal.
![](/images/paris-pont-des-arts-cadeados.png)
### Gerando as chaves
Gerar as chaves é muito fácil! Comece digitando esse comando abaixo:
```
gpg --gen-key
```
Essa mensagem aparecerá:
```
gpg (GnuPG) 2.0.24; Copyright (C) 2013 Free Software Foundation, Inc.
This is free software: you are free to change and redistribute it.
There is NO WARRANTY, to the extent permitted by law.

Por favor selecione o tipo de chave desejado:
   (1) RSA and RSA (default)
   (2) DSA and Elgamal
   (3) DSA (apenas assinatura)
   (4) RSA (apenas assinatura)
Sua opção?
```
Escolha a 1 mesmo (ou dê enter). Esse é o padrão e funciona muito bem! A resposta do terminal será essa:
```
Sua opção? 1
RSA keys may be between 1024 and 4096 bits long.
What keysize do you want? (2048)
```
Esse é o tamanho da chave. Se você não estiver criptografando coisas ultra-mega-sinistramente secretas use o padrão mesmo. Digite 2048 ou dê enter.
```
Por favor especifique por quanto tempo a chave deve ser válida.
         0 = chave não expira
      <n>  = chave expira em n dias
      <n>w = chave expira em n semanas
      <n>m = chave expira em n meses
      <n>y = chave expira em n anos
A chave é valida por? (0)
```
Agora vamos escolher a validade da chave. Mais uma vez, vá com o padrão e dê enter.
```
Key does not expire at all
Is this correct? (y/N)
```
Confirme com um `y`.

Agora o GPG vai pedir suas informações de usuário:
```
GnuPG needs to construct a user ID to identify your key.
Nome completo:
Endereço de correio eletrônico:
Comentário:

Muda (N)ome, (C)omentário, (E)ndereço ou (O)k/(S)air?
```
Preencha cada um dos campos e dê enter ao fim. Depois, se tudo estiver certinho, de O e você precisará colocar a senha duas vezes.

> Crie uma senha com pelo menos 16 dígitos alfanuméricos, com símbolos, acentos, maiúsculas, etc. Guarde a senha bem.

Agora as chaves serão criadas. Olha o resultado:

```
Precisamos gerar muitos bytes aleatórios. É uma boa idéia realizar outra                                                                                                                                                                                                       
atividade (digitar no teclado, mover o mouse, usar os discos) durante a                                                                                                                                                                                                        
geração dos números primos; isso dá ao gerador de números aleatórios                                                                                                                                                                                                           
uma chance melhor de conseguir entropia suficiente.                                                                                                                                                                                                                            
Precisamos gerar muitos bytes aleatórios. É uma boa idéia realizar outra                                                                                                                                                                                                       
atividade (digitar no teclado, mover o mouse, usar os discos) durante a                                                                                                                                                                                                        
geração dos números primos; isso dá ao gerador de números aleatórios                                                                                                                                                                                                           
uma chance melhor de conseguir entropia suficiente.                                                                                                                                                                                                                            
gpg: key 7066F886 marked as ultimately trusted                                                                                                                                                                                                                                 
chaves pública e privada criadas e assinadas.                                    
```

Antes de continuarmos, você pode listar todas as chaves disponíveis no seu chaveiro:
```
gpg --list-keys
```

### Exportar chaves públicas
Agora, antes de você poder receber mensagens criptografadas de seus amigos é necessário que eles tenham sua chave pública. Para isso, vamos exportar a chave pública que criamos no passo anterior.
```
gpg --armor --output minha_chave_publica.asc --export bruce
```
Veja que no exemplo acima usei a opção `--armor` para criar o arquivo em formato ASCII. Além disso, é possível usar o email ao invés do nome depois do `--export`. Esse comando vai gerar um arquivo chamado `minha_chave_publica.asc` que poderá ser enviado para seus amigos ou disponibilizados nos diversos chaveiros online

Eu sugiro publicar sua chave pública no MIT. A universidade vai distribuir sua chave e ela estará nos principais servidores de chaves em alguns dias. Para publicar sua chave você precisará do ID da chave.
```
gpg --list-keys
pub   2048R/7066F886 2016-08-17
uid       [ultimate] Delchi Bruce (delchibruce.com) <db@delchibruce.com>
sub   2048R/4A88E498 2016-08-17
```
No caso acima o ID é `4A88E498`. Agora é só publicar:
```
gpg --keyserver pgp.mit.edu --send-key FCB70A46
```
![](/images/chave.png)
### Importando
Agora vamos fazer o outro lado da operação: importar a chave recebida. Você recebeu uma chave pública ou pegou no servidor e agora quer importá-la para poder criptografar arquivos ou mensagens e mandar para o dono da chave. Comece assim:
```
gpg --import ~/bruce/minha_chave_publica.asc
```
Atente para o fato que eu importei uma chave exportada com a opção `--armor`. Se for criada sem essa opção ou usando outra ferramenta, ela provavelmente terá a extensão `.gpg`.

Para buscar no servidor é só:
```
gpg --keyserver pgp.mit.edu --search-key db@delchibruce.com
```

Uma vez importada a chave, pode mandar bala e usá-la para criptografar os dados.

## Criptografando arquivos
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
## Criptografando e-mails
Agora, uma das coisas mais importantes: **comunicação protegida**. Existem diversas ferramentas disponíveis para se mandar (e receber) emails criptografados. Se você não quiser a opção abaixo, que é na linha de comando, sugiro o combos [thunderbird](https://www.mozilla.org/pt-BR/thunderbird/) com [enigmail](https://www.enigmail.net/index.php/en/) ou [gmail](gmail.com) com [mailvelope](https://www.mailvelope.com). Tanto o enigmail quanto o mailvelope usam o GPG para gerir as chaves e criptografar.

Mas eu uso esse método aqui: crio as mensagens e as criptografo no terminal. Fácil, fácil.
```
gpg -aer bruce
```
Aí escreva a mensagem, no terminal mesmo. Quando terminar de escrever sua mensagem, pressione `crtl+d`. Você terá, como resultado, uma mensagem parecida com essa:

```
-----BEGIN PGP MESSAGE-----
Version: GnuPG v2

hQEMA9V3kDz8twpGAQgAvMo9RQR4wgD4uFU3BaeLyQ1Rj5LSOTgCf01DmZY8TREt
WiyxTT5/lzns2HtIIciIygE7YQnnj8H/rzNAITshxk/w3wfNaKNaVfFLT3p5AEJx
AB944HJcpadgsR3JQ8ZdkbxSThjzq7tPzEfFWFHE687qPSB4U9IteouREGXtSxIE
sV9xwcDCIBZUkzEZwzJJtuWtnrzufKrRqmuyvMOwAjWHofyyPBKjaHZ66SYkeUiT
ZZyBuVA79vnGv5OvCKsBnaNSsEaEuiyjHKULXQM0c6TPEIHvXBaiOXy3G/qog2ls
5tbIYR4TrAyAdUCXO9+/6zLmVg2XBshJSDwCpl/HcNJ3ARIqaAddIqMwe5no+L6a
mHAAiRcY4r1HOUahoQAIfiHj9d0ag823EfLubAYMNPqc3Kzyy8ynLrCydJHw5Mec
7SpkMSF8PmKO4F2VJG6tV/tJhWpvCIXQyLDdqMO+2YMmVncqf4a0RahDmUh27anH
pB0e9MF6Z44=
=dJjU
-----END PGP MESSAGE-----
```

Agora é só colar esse texto no email e mandar pro dono da chave. Lá ele usará a chave privada para descriptografar a mensagem. E o legal é que ele poderá fazer isso em qualquer plataforma, app, software ou interface que ele quiser.
No terminal é assim:

```
gpg -d
```
Dê enter.
```
COLE O TEXTO criptografado
```
Dê enter.
```
coloque a senha
```
A mensagem original vai aparecer.

Fácil, seguro e muito, muito maneiro!!

espero que se protejam! e qualquer dúvida, comenta aí emabaixo!

delchi bruce

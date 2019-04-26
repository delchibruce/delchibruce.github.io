---
title: Montando um sistema de controle de versão (e backup) de documentos
date: 2017-01-24 14:40:38
categories:
- Aplicativos
tags:
- backup
- controle de versão
- eficiência
- tranquilidade
- git
thumbnail: /images/pc_destruido.jpg
---
![](/images/pc_destruido.jpg)
Que fazer backup é importante, todo mundo sabe!  Principalmente quem já perdeu arquivos inportantes por causa de um vírus ou de de um HD queimado.

O melhor a fazer é ter um backup automático, incremental e imediato, que garanta que seus arquivos estejam protegidos contra catástrofes computacionais.

Mas, às vezes, só o [backup](/2016/09/18/Nao-perca-tempo-nem-arquivos/) não satisfaz. Às vezes queremos um backup que controle a versão dos documentos. Todas as alterações, por menor que sejam, são registradas e é possível, inclusive, reverter o arquivo a cada uma delas.

<!-- more -->

Essa ferramenta é especialmente interessante para equipes, mas eu uso no meu dia-a-dia, só pra mim. Espero que seja útil!

## A importância do backup
Não precisamos discorrer muito sobre a importância do backup. Quem já perdeu o último capítulo da dissertação de mestrado na semana de entregar o texto final sabe como isso pode te derrubar e deixar em frangalhos.

Hoje, muito mais que no passado, usamos a nuvem e temos muitas formas de guardar nossos arquivos. Documentos vão para o dropbox, imagens para o google fotos, etc, etc. Mas, muitas vezes, esses serviços não são suficientes para guardarmos tudo direitinho. E nem todos permitem mater versões diferentes dos arquivos.

## Os benefícios do controle de versão
Aí vem a pergunta: por que fazer controle de versão dos documentos?

A resposta não é simples. No mundo de hoje, com a quantidade de recursos e informações disponíveis, gastamos bastante tempo buscando, arrumando e sistematizando as informações que nos são importantes.

Concurseiros sabem muito bem como tudo isso funciona. Gasta-se tempo buscando textos, legislação, questões, aulas, etc. Aí dá uma pala no sistema operacional, ou vem um maldito vírus, e detona tudo. **Começar de novo não é muito agradável.**

Então, ao fazer seu repositório de documentos, conceitos, textos, aulas e anotações, criar um backup e manter um controle de versão pode ser a decisão que te faz não desistir quando der pau em tudo. **Espero que nunca dê pau no seu HD e que você nunca perca as informações importantes. Mas, se perder, tenha um plano de contigência.**

Controlar a versão do documento permite voltar atrás, mesclar dois documentos, reverter mudanças, etc.

Nem todos os documentos que você escreve precisam ter a versão controlada. Se você, no entanto, está escrevendo um artigo, um TCC, uma dissertação ou tese ou ainda, [criou uma grande biblioteca de informações para estudar para concurso](/2017/01/13/Usando-Mapas-Mentais-no-estudo-para-concursos/), **você TEM que fazer uum controle de versão bem ajustadinho**.

E o jeito mais fácil de fazer isso é com o `git`.

## O que é git?
O `git` é uma ferramenta de controle de versão feita por Linus Torvalds, o criador do linux. Foi criado para controlar alterações em código fonte do próprio Linux. Apesar de não ser o único sistema desses por aí é o mais completo pelo método que utiliza.

A ideia é simples: existem repositórios de dados que agrupam as informações. Todos os diretórios que usam git são repositórios completos, com todos os arquivos, controle de versão e histórico completo de alterações. **Não é necessário estar conectado à internet para mudar a versão do repositório e acessar documentos como estavam antes da mudança que você fez**.

Agora, se você tiver um servidor próprio, pode ligar seu repositório local ao repositório do servidor. Pronto! Mas, se como a maioria das pessoas, você não tem um servidor, pode usar serviços que fornecem o repositório remoto. As mais conhecidas são [github](https://github.com), [bitbucket](https://bitbucket.org) e [gitlab](https://about.gitlab.com).

O mais conhecido é o github, que é ótimo para projetos de código aberto, mas não oferece repositório privado gratuito. O bitbucket oferece e, por isso, é o que eu uso!

## Mas eu já faço isso com o Dropbox!
Existem outros serviços que oferecem algum tipo de controle de versão, como o dropbox e o google drive. E talvez isso já seja suficiente para você.

Se você estiver satisfeito com eles, show!

**Se não tiver, continue lendo!**

## Usando o bitbucket
Você vai precisar de:
-   ter o `git` instalado;
-   uma conta no [bitbucket](https://bitbucket.org);
-   entender os termos e conceitos do `git`.

Se você usa windows ou mac, provavelmente precisará baixar o git. Olha ele [aqui](https://git-scm.com/downloads). Quem tem linux já tem git!

### Termos e expressões
Comecemos com os principais conceitos do `git`.
-   `repositório` - a coleção de documentos, commits, branches e históricos. Sua biblioteca de dados;
-   `local` - que está no seu computador;
-   `remoto` - que está no servidor do bitbucket/github/gitlab;
-   `origin` - esse é o repositório remoto, no servidor do bitbucket/github/gitlab;
-   `branch` - um galho de desenvolvimento independente;
-   `master` - o branch padrão. Seus documentos estão nesse galho;
-   `pull` - esse comando puxa as mudanças do repositório remoto para o local
-   `commit` - é uma "versão" dos documentos ou repositórios. É quando você salva as alterações no repositório master;
-   `push` - envia as mudanças para o repositório remoto;
-   `head` - é o estado atual do seu repositório. Como ele está nesse momento;
-   `clone` - uma cópia do repositório remoto, feita localmente.

### Configurando tudo
Depois de abrir sua conta no bitbucket, crie um projeto e, depois, um repositório. Pode ser "material de estudo", "dissertação", ou "documentos importantes". Ou outra coisa qualquer!

Criei o projeto "estudo" e o repositório "material_de_estudo"

Aí, no console do seu computador:
```
git clone bitbucket.og/git/estudo/material_de_estudo.git
```
> OBS: no Windows você precisa abrir o "Command Prompt" ou "Prompt de Comando" e no Mac, o "Terminal".

![](/images/command_promt_do_windows10.png)

Esse comando vai criar uma pasta chamada "material_de_estudo". Crie uns documentos nessa pasta. Aí, faça o envio pro repositório remoto:
```
cd <material_de_estudo>
git add .
git commit -m "mandando todos documentos"
git push origin master
```

Pronto! Agora os documentos estão no repositório remoto. **Se você usa mais de uma máquina é só entrar na outra e clonar o repositório.** Vai baixar tudo: documentos, imagens e o histórico de alterações!

Agora, quando criar novos documentos você tem que adicioná-los ao repositório. Fácil:
```
git add nome_do_documento.doc
```
Alternativamente, você pode adicionar todos os arquivos de uma só vez usando o `git add .`

Mande para o repositório remoto!
```
git commit -m "enviando nome_do_documento.doc"
git push origin master
```
Aí o `nome_do_documento.doc` vai estar disponível no repositório remoto. Nos outros computadores rode um `pull`:
```
git pull
```

Fácil, não é?

### Verificando versões
Para ver todos os commits que você fez é só usar o `git log`. As mensagens que você colocou depois dos `git commit -m` vão aparecer todas. Assim:
```
commit 1c8dab31faf28738b55421f384c6783fb3df79c4
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Mon Jan 23 10:32:39 2017 -0200

    ajustes e sitemap

commit 38ca758c0766bd1ecf9219b79483626bfa82419a
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Fri Jan 13 12:46:52 2017 -0200

    novo post: mapas mentais

commit 57b4b73a3eb1f0930da80255a69d22cc5f9cd340
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Sat Dec 31 21:06:02 2016 -0200

    correção de erros

commit d96612cd561d3962ba372502b6f589285c480749
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Sat Dec 31 14:01:08 2016 -0200

    post: resoluções de ano novo

commit 09842a1c8424f029f5599813bd357e99a3a77efc
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Sun Dec 25 11:06:09 2016 -0200

    db.json

commit 81db1371d210a763133f1dabf58f64b676e7e5c5
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Fri Dec 23 19:51:05 2016 -0200

    novo artigo: o que fazer se não tem tempo para estudar

commit 04f31404e59063f14e11a5f59cc29c40ea57e916
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Tue Dec 20 11:22:26 2016 -0200

    novo artigo 'tempo para estudar' e correções

commit c587ec0e2c0d03143a1ae155f9818b38edd7658f
Author: Delchi Bruce <delchi.bruce@gmail.com>
Date:   Wed Dec 7 16:25:07 2016 -0200
```

Para ver as alterações em um arquivo é só usar:
```
git diff commit_1_^^ commit_2 -- /caminho/para/o/arquivo.doc
```
Mude `commit_1_` e `commit_2` pelos IDs dos commits mostrados pelo `git log`.

Por fim, para voltar atrás e **desfazer** um commit é só usar o `revert`:
```
git revert ID_do_commit
```
Claro que `ID_do_commit` deve ser substituído pelo ID do commit que você quer desfazer. Lembre-se que a lista de IDs pode ser visualizada com o `git log`.

Você consegue ver todos os arquivos no site do bitbucket.

Agora você tem um sistema de backup e de controle de versões!

Parece complicado, mas não é! E é útil pra caramba!! Precisa de ajuda? Diz aí!

<div style="padding: 5px 20px; margin: 5px; background: #F0F3F1;"><img src="/images/new_eu_round_pad.png" style="float:left;width:115px;height:115px;">**Delchi Bruce** estudou Relações internacionais e é professor de cursinho desde 2008. É apaixonado por formas de melhorar a produtividade e ser mais eficiente. É gestor de conteúdo do [Mapa da Prova](http://mapadaprova.com.br), ferramenta de estudo para concurseiros. Usa linux [(openSUSE)](http://opensuse.org) desde 1999 e faz trade de [bitcoin](http://bitcoin.org) todo dia.
Você pode entrar em contato [por aqui](/contato/).</div>

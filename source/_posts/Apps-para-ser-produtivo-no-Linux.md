---
title: Apps para ser produtivo no Linux
date: 2016-06-23 20:01:01
tags:
  - produtividade
  - nuvem
  - apps
  - programas
categories:
  - Linux
  - Produtividade
thumbnail: /images/produtividade.png
---

![](/images/produtividade.png)

Olá pessoal,

como sabem, sou um usuário inveterado de Linux desde o milênio passado. Atualmente uso o OpenSUSE Leap 42.1, que é uma **versão bem estável** de um SO super interessante. Como ambiente uso o KDE Plasma 5, que é amigável e **bem configurável**, apesar de mais pesado que outros por aí.

>Uso essa máquina para tudo: trabalhar, preparar aula, me divertir, assistir filmes e mesmo Netflix. Tudo.

Aí, de vez em quando, vem alguém e me pergunta: "como você consegue trabalhar quando não tem os programas 'normais'". Aí eu disparo, rapidamente: "trabalho muito bem!"

E olha o porquê:
<!-- more -->

Primeiro: dá pra fazer muita coisa com os apps na núvem. No(s) trabalho(s) eu uso muito o Google Drive e sem as tabelas que fizemos lá, estaria perdido. Vocês podem ver que sempre que preciso fazer uma tabela, uso o Spreadsheets e pras minhas aulas eu uso o Presentations, ambos da Google. O LibreOffice é fera e tals, mas os xmls da microsoft ainda ficam cag$d@s. Também uso muito o appear.in pra reuniões, já que o skype é da microsof e sempre que podemos, evitamos apps da M$. Então, usamos e abusamos da nuvem.

Mas, a núvem nem sempre consegue substituir todas as funções necessárias e por isso, listo esses apps aí. Sem eles eu não trabalharia. Nem um pouco!

## Tomar notas:

Eu uso dois programas para fazer anotações. Já usei muito o Evernote, mas alguma coisa aconteceu que nos separamos. Deve ter sido a dificuldade de rodá-lo nativamente no Linux. De qualquer maneira, uso dois excelentes apps: o Leanote e o MyNotex.

O leanote permite criar cadernos e notas (em texto e em markdown) e acessá-los de qualquer lugar pela interface web. Eu ainda uso muito na versão local (sem sincronização), fazendo meus próprios backups, mas acredito que eles estão se afastando desse modelo e passando a usar somente a versão sincronizada.

![](/images/leanote_vazio.png)

O [MyNotex](https://sites.google.com/site/mynotex/) é mais parrudo e também surgiu com essa divisão de cardenos e notas. Mas o desenvolviemto, feito pelo Massimo Nardello, é bem ativo e é possível, agora, criar notas criptografadas, adiicionar anexos e controlar atividades (tipo lista de tarefas), com controle de tempo e tudo.

![](/images/mynotex.png)
Eu gosto tanto desse programa que ajudei com a versão em português dele!  =D

Uso bastante os dois: o [leanote](http://leanote.org) para notas mais rápidas, que não precisam de muita informação adiconal, como imagens, anexos, etc; enquanto no MyNotex mantenho registros detalhados do trabalho, livros emprestados, log de ligações, etc. Eu percebi que sou um anotador compulsivo.

## Organizar Ideias

Anotar é ótimo! Mas, em alguns momentos, as ideias precisam ser organizadas, certo? Eu organizo tudo criando mapas mentais com o [Freemind](http://freemind.sourceforge.net/wiki/index.php/Main_Page). Aqueles que já assistiram minhas aulas (presenciais) sabem que crio mapas mentais o tempo todo. E o Freemind permite criar mapas simples e super complexos.

![](/images/mm_freemind.png)

Depois de criar os mapas, ainda posso transformá-los em markdown ou em applets para colocar em páginas, como esse aí embaixo.

## Backup, backup, backup -> Não esqueça nunca de fazer backup!!

Todo mundo usa o Dropbox, certo? Esse negócio já me salvou umas 4 ou 5 vezes. É bom de mais!! Ele sincroniza tudo direitinho. Mas nos dá pouco espaço na versão gratuita e, por isso, eu também uso o Mega Sync, que funciona até direitinho no linux e disponibiliza 50 GB de backup.

Só que não confio apenas nesses serviços. Algumas coisas devem estar bem guardadas e de fácil acesso, certo? Então, para isso, eu uso o [Synkron](http://synkron.sourceforge.net), que permite sincronizar minhas coisas mais importantes com um HD de 1 TB para backup gerais e ainda uns pendrives com informações específicas.

![](/images/synkron_topo.png)

Backup, galera!

## Senhas seguras e em segurança

Você não usa as mesmas senhas para tudo, certo? **CERTO??**

Bem, se usa, não deveria. Eu não uso! Além de usar senha para cada coisa, gero as senhas no computador também.

Já gerei muita senha usando a linha de comando, mas agora estou usando o [keepassx](http://www.keepassx.org/) que gera as senhas e armazena todas elas, organizadinhas.

Se quiser usar a linha de comando para gerar seu pwd, tenta isso aqui:

```
date +%s | sha256sum | base64 | head -c 32 ; echo
```
ou usando o openssl:

```
openssl rand -base64 20
```
Mas, para organizar, lembrar e deixar tudo supimpa, usa o keepassx.

![](https://www.keepassx.org/wp-content/uploads/2016/02/kpx2_main.png)

>Então, pessoal, essa estória que no linux não dá pra trabalhar direito é balela! E o melhor de tudo: é confiigurável, ajustável, inteligente, software livre e **não pega vírus**.

**Você usa linux? Quais os programas que te fazem produtivo?

Usa windows ou mac? Sem erro! Comenta aí também!!

abraços produtivos!**
db

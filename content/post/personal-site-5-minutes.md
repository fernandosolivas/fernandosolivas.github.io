+++
title = "Criando seu site em 5 minutos e de graça"
description = "Vamos utilizar o Hugo, framework de temas em Go."
date = "2020-02-29"
categories = [
    "Development",
    "golang",
]
+++

Quem nunca quis ter um site, currículo ou até mesmo um portfólio todo customizado, com sua cara e só seu? Muitos usam o Wordpress pra atingir esses pontos mas acaba demandando um servidor para rodar ou pagar para que funcione. Aqui, vou explicar como utilizei o [Hugo](https://gohugo.io/) para fazer este site em menos de 5 minutos utilizando, [Github Pages](https://pages.github.com/) e um editor de texto, no meu caso o [VSCode](https://code.visualstudio.com/).

## Github Pages
Caso você não conheça o [Github](https://github.com/), ele é um versionador de controle de código que utiliza o [Git](https://git-scm.com/) como sistema. Caso não conheça um dos dois, aconselho leitura posterior para aprofundamento. Por ora, iremos precisar instalar o Git em sua máquina local e criar uma conta no [Github](https://github.com)

### Instalando o Git
Para instalar o Git é bem simples e você pode escolher qual versão você quer. Seja [Windows](https://git-scm.com/download/win), [Mac OS X](https://git-scm.com/download/mac) ou [Linux](https://git-scm.com/download/linux).

Após instalar, será necessário realizar algumas configurações básicas. Abra o terminal do seu sistema operacional e execute o comando abaixo.

```bash
$ git config --global user.email "seuemail@dominio.com"

$ git config --global user.name "Seu Nome"
```

Pronto, seu cliente Git está configurado!

### Criando sua conta no Github
Acesse o [Github](https://github.com), clique em **Sign Up**  para criar uma conta.

![signup](/github-signup.png)

Em seguida crie sua conta preenchendo o formulário e selecione o plano gratuito do Github.

![gh-create-account-form](/gh-create-account-form.png)

E... Shazaam!! Sua conta está criada, mas não feche o Github agora, já já iremos precisar dele! :)

## Go
Instale  o [Go](https://golang.org/dl/) na sua máquina antes de prosseguir. Iremos precisar dele para instalar e executar o próximo passo.

## Hugo

Hugo é um framework em [go][] feito para criar sites através de templates, abstraindo o máximo possível de lógica. Você pode criar um template e disponibilizar na comunidade para que outras pessoas possam usar. Logo, outras pessoas (bem legais) já criaram templates para que possamos usar. Se quiser dar uma olhada nas opções, você pode encontrar clicando [aqui](https://themes.gohugo.io/).

Lembre-se, antes de instalar/rodar o Hugo, você precisará ter o [go][] instalado!

### Iniciando nosso site

A primeira coisa que a gente vai precisar fazer é criar um repositório no Github. Um repositório é um "diretório" na nuvem com histórico e apartir desse histórico é possível fazer um monte de coisa. Então, na sua página inicial do Github, clique no botão de verde no canto esquerdo pra criar um novo repositório, como a imagem abaixo.

![gh-create-repo](/gh-create-repository.png)

No formulário para criar um novo repositório, você *deve* criar um repositório no formato _nomedasuaconta.github.io_, caso contrário não irá funcionar.

![gh-create-repo-pages](/gh-create-repository-pages.png)

Em seguida, você será redirecionado para uma página com um tutorial para você começar a modificar seu repositório.

![gh-create-repo-feedback](/gh-create-repository-feedback.png)

Nós vamos utilizar o primeiro exemplo e iremos começar a realizar nossas edições com o Hugo.

## Utilizando nosso repositório como site

Agora, abra a linha de comando do seu sistema operacional e execute o comando

```bash
$ git clone https://github.com/<nomedoseurepositorio>/<nomedoseurepositorio>.github.io.git
```

O git por padrão irá solicitar sua senha de acesso ao Github. Relaxa, isso é normal. Digite sua senha e logo você verá que uma pasta foi criada com o _nomedoseurepositorio_.

Entre nessa passata e iremos começar a realizar a mágica.

## Instalando o Hugo

Primeiro nós vamos instalar o Hugo, você precisará ter instalado o [go][]1.11+ e o Git. Esses são os únicos pré requisitos. Agora execute no seu terminal uma dessas opcões.

#### Mac OS
```bash
$ brew install hugo
```

#### Linux
```bash
$ brew install hugo
```
*Caso não tenha o [Homebrew](https://docs.brew.sh/Homebrew-on-Linux) instalado no Linux, instale antes.

#### Windows
```bash
$ choco install hugo -confirm
```
*Caso não tenha o [Chocolatey](https://chocolatey.org/) instalado no Windows, instale antes.

## Rodando Hugo
Após a instalação ter sido concluída, nós vamos conseguir executar no terminal o seguinte comando para criarmos nossos site.

```bash
$ hugo new site . --force
```

O argumento _--force_ é necessário pois o diretório já existe e o Hugo para confirmamos que queremos escrever nessa pasta de destino. Após executar você deve ter a seguinte estrutura:

```bash
$
├── archetypes
│   └── default.md
├── config.toml
├── content
├── data
├── layouts
├── static
└── themes
```

#### Escolhendo um tema
Essa é uma estrutura base do framework que não precisamos nos preocupar agora. Vamos escolher um tema na [página da comunidade](https://themes.gohugo.io/). Eu irei utilizar o tema [Cactus](https://themes.gohugo.io/cactus/).

Na raiz do nosso projeto, vamos executar os dois comandos abaixo:
```bash
$ cd themes
$ git clone https://github.com/digitalcraftsman/hugo-cactus-theme.git
```
Isso fará com que a gente baixe o tema Cactus para nosso repositório e possamos customizar ele. Repare que na raiz do seu projeto, existe um arquivo chamado `config.toml`. Iremos adicionar uma linha indicando qual tema iremos usar. O arquivo deverá ficar assim:

```bash
baseURL = "http://example.org/" <- modifique esse valor para https://<seurepositorio.github.io>
languageCode = "en-us"
title = "My New Hugo Site"
theme = "hugo-cactus-theme"
```

Agora, iremos executar na raiz do projeto o seguinte comando `hugo server`. Isso fará rodar um servidor local para que você possa ver uma prévia do seu site. No momento, só teremos um exemplo pois ainda não alteramos nada. Se você acessar pelo navegador a url: http://localhost:1313 verá uma página similar a essa

![boilerplate](/cactus-boilerplate.png)

Ótimo, agora já temos algo visual! Precisamos agora customizar conforme preferirmos para deixar nosso site mais bonito. Existem muitas formas de customizar o tema Cactus. Podemos ver mais detalhes clicando [aqui](https://github.com/digitalcraftsman/hugo-cactus-theme).

#### Customizando nosso tema

Primeira alteração que iremos fazer é copiar para a raiz do nosso projeto, um arquivo chamado `config.toml`(sim, igual o seu) que está em ./themes/exampleSite. Esse arquivo já contém muitas opções de configuração e lá você já pode alterar o nome, título, descrição e outros.

Nós iremos configurar juntos uma seção, que é a de _social_. No fim do arquivo iremos adicionar o seguinte conteúdo:
```bash
[social]
    twitter  = "https://www.twitter.com/fernandosolivas"
    github   = "https://www.github.com/fernandosolivas"
	linkedin = "https://www.linkedin.com/in/fernandosoliva/"
	medium = "https://medium.com/fernandosolivas"
```

E escolha uma foto bem legal para colocar no centro do página, você precisará movê-la para ./themes/static/images/avatar.png. Tem que ter o mesmo nome, senão não irá funcionar.

No fim, teremos algo assim. 

![cactus-boilerplate-edited](/cactus-boilerplate-edited.png)

#### Adicionando mais informações

Agora, nossa página está um pouco crua ainda, então vamos adicionar uma página com uma descrição sobre a gente. O framework Hugo espera que todas as páginas criadas estejam dentro da pasta <raizdoprojeto>/content, então vamos criar uma pasta chamada about e arquivo chamado `_index.md` dentro da nova pasta criada.

```bash 
├── archetypes
│   └── default.md
├── config.toml
├── content
│   └── about
│       └── _index.md
...
```

Você pode não estar familiarizado mas esse sufixo de arquivo `.md` representa um arquivo do tipo [Markdown](https://en.wikipedia.org/wiki/Markdown) e [aqui](https://github.com/adam-p/markdown-here/wiki/Markdown-Cheatsheet) tem uma lista de exemplos de como se pode usar.

Vamos começar editando o arquivo e adicionando o seguinte texto no topo do arquivo:

```bash
+++
date = "2015-06-20T14:02:37+02:00"
title = "Sobre"
hidden = false
menu = "main"
+++
```

Cada item é auto explicativo mas vamos dar uma passada por eles:
- Date: Data de publicação da página
- Title: Título / Nome da página
- Hidden: Se deverá aparecer no menu
- Menu: Em qual menu ele deverá estar relacionado

Depois disso só nos resta começar a escrever, o meu arquvio `_index.md` fica assim:

```bash
+++
date = "2015-06-20T14:02:37+02:00"
title = "Sobre"
hidden = true
menu = "main"
+++

Software Developer na [Calindra](https://calindra.tech/), Coordenador do [Meetup GopheRio](https://www.meetup.com/pt-BR/GopheRio), voluntário no [Poder do Voto](https://poderdovoto.org), Fundador da [Simbah](https://www.simbahdc.com/), escritor nas horas vagas e pai de uma menina incrível e marido da melhor ilustradora do país [link](https://www.instagram.com/aruizilustra/)!

Gosto de desenvolver em muitas linguagens e utilizar cada uma delas para seu propósito correto. As linguagens que tenho mais conhecimento são: Java, Go, Python, JavaScript (TypeScript).

Entusiasta de TDD, Clean Code, CI e CD e boas práticas de desenvolvimento.

***

```

#### Disponibilizando nosso site na internet

Agora, de volta ao Git. Vamos mandar as alterações que fizemos nos arquivos lá para o repositório que criamos. Para isso iremos executar os seguintes comandos na raiz do projeto.

```bash
$ hugo -d .
$ git add *
$ git commit -m "Enviando site com descrição"
$ git push origin master
```

Agora se você acessar https://<seurepositorio.github.io> você verá seus site online, de graça e sem precisar escrever 1 linha de código, somente textos.

#### Conclusão

Existem algumas formas de se criar um site de forma gratuita sem precisar escrever código, como wix.com, wordpress* e outros. O Hugo é uma boa ferramenta pra quem já programa ou está começando pois requer pouco (ou nenhum) conhecimento em desenvolvimento e ferramentas para já conseguir fazer um site.

O Hugo tem vários temas incríveis e fácis de usar, se quiser ir tentando modificar o seu, vai fundo!

Espero que tenha gostado do post e mande suas dúvidas no meu twitter.

Até a próxima!

[go]: <http://golang.org/>
[gohtmltemplate]: <http://golang.org/pkg/html/template/>
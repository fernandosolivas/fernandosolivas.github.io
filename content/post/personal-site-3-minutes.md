+++
title = "Criando seu pessoal em 3 minutos e de graça"
description = "Vamos utilizar o Hugo, framework de temas em Go."
date = "2020-02-29"
categories = [
    "Development",
    "golang",
]
+++

Quem nunca quis ter um site, currículo ou até mesmo um portfólio todo customizado, com sua cara e só seu? Muitos usam o Wordpress pra atingir esses pontos mas acaba demandando um servidor para rodar ou pagar para que funcione. Aqui, vou explicar como utilizei o [Hugo](https://gohugo.io/) para fazer este site em menos de 3 minutos utilizando, [Github Pages](https://pages.github.com/) e um editor de texto, no meu caso o [VSCode](https://code.visualstudio.com/).

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
$ git add *
$ git commit -m "Enviando site com descrição"
$ git push origin master
```

## Basic Syntax

Go lang templates are html files with the addition of variables and
functions. 

**Go variables and functions are accessible within {{ }}**

Accessing a predefined variable "foo":

    {{ foo }}

**Parameters are separated using spaces**

Calling the add function with input of 1, 2:

    {{ add 1 2 }}

**Methods and fields are accessed via dot notation**

Accessing the Page Parameter "bar"

    {{ .Params.bar }}

**Parentheses can be used to group items together**

    {{ if or (isset .Params "alt") (isset .Params "caption") }} Caption {{ end }}


## Variables

Each go template has a struct (object) made available to it. In hugo each
template is passed either a page or a node struct depending on which type of
page you are rendering. More details are available on the
[variables](/layout/variables) page.

A variable is accessed by referencing the variable name.

    <title>{{ .Title }}</title>

Variables can also be defined and referenced.

    {{ $address := "123 Main St."}}
    {{ $address }}


## Functions

Go template ship with a few functions which provide basic functionality. The go
template system also provides a mechanism for applications to extend the
available functions with their own. [Hugo template
functions](/layout/functions) provide some additional functionality we believe
are useful for building websites. Functions are called by using their name
followed by the required parameters separated by spaces. Template
functions cannot be added without recompiling hugo.

**Example:**

    {{ add 1 2 }}

## Includes

When including another template you will pass to it the data it will be
able to access. To pass along the current context please remember to
include a trailing dot. The templates location will always be starting at
the /layout/ directory within Hugo.

**Example:**

    {{ template "chrome/header.html" . }}


## Logic

Go templates provide the most basic iteration and conditional logic.

### Iteration 

Just like in go, the go templates make heavy use of range to iterate over
a map, array or slice. The following are different examples of how to use
range.

**Example 1: Using Context**

    {{ range array }} 
        {{ . }}
    {{ end }}

**Example 2: Declaring value variable name**

    {{range $element := array}} 
        {{ $element }} 
    {{ end }}

**Example 2: Declaring key and value variable name**

    {{range $index, $element := array}}
        {{ $index }} 
        {{ $element }} 
    {{ end }}

### Conditionals 

If, else, with, or, & and provide the framework for handling conditional
logic in Go Templates. Like range, each statement is closed with `end`.


Go Templates treat the following values as false: 

* false
* 0 
* any array, slice, map, or string of length zero

**Example 1: If**

    {{ if isset .Params "title" }}<h4>{{ index .Params "title" }}</h4>{{ end }}

**Example 2: If -> Else** 

    {{ if isset .Params "alt" }} 
        {{ index .Params "alt" }}
    {{else}}
        {{ index .Params "caption" }}
    {{ end }}

**Example 3: And & Or**

    {{ if and (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")}}

**Example 4: With**

An alternative way of writing "if" and then referencing the same value
is to use "with" instead. With rebinds the context `.` within its scope,
and skips the block if the variable is absent.

The first example above could be simplified as:

    {{ with .Params.title }}<h4>{{ . }}</h4>{{ end }}

**Example 5: If -> Else If** 

    {{ if isset .Params "alt" }} 
        {{ index .Params "alt" }}
    {{ else if isset .Params "caption" }}
        {{ index .Params "caption" }}
    {{ end }}

## Pipes

One of the most powerful components of go templates is the ability to
stack actions one after another. This is done by using pipes. Borrowed
from unix pipes, the concept is simple, each pipeline's output becomes the
input of the following pipe. 

Because of the very simple syntax of go templates, the pipe is essential
to being able to chain together function calls. One limitation of the
pipes is that they only can work with a single value and that value
becomes the last parameter of the next pipeline. 

A few simple examples should help convey how to use the pipe.

**Example 1 :**

    {{ if eq 1 1 }} Same {{ end }}

is the same as 

    {{ eq 1 1 | if }} Same {{ end }}

It does look odd to place the if at the end, but it does provide a good
illustration of how to use the pipes.

**Example 2 :**

    {{ index .Params "disqus_url" | html }}

Access the page parameter called "disqus_url" and escape the HTML.

**Example 3 :**

    {{ if or (or (isset .Params "title") (isset .Params "caption")) (isset .Params "attr")}}
    Stuff Here
    {{ end }}

Could be rewritten as 

    {{  isset .Params "caption" | or isset .Params "title" | or isset .Params "attr" | if }}
    Stuff Here 
    {{ end }}


## Context (aka. the dot)

The most easily overlooked concept to understand about go templates is that {{ . }}
always refers to the current context. In the top level of your template this
will be the data set made available to it. Inside of a iteration it will have
the value of the current item. When inside of a loop the context has changed. .
will no longer refer to the data available to the entire page. If you need to
access this from within the loop you will likely want to set it to a variable
instead of depending on the context.

**Example:**

      {{ $title := .Site.Title }}
      {{ range .Params.tags }}
        <li> <a href="{{ $baseurl }}/tags/{{ . | urlize }}">{{ . }}</a> - {{ $title }} </li>
      {{ end }}

Notice how once we have entered the loop the value of {{ . }} has changed. We
have defined a variable outside of the loop so we have access to it from within
the loop.

# Hugo Parameters 

Hugo provides the option of passing values to the template language
through the site configuration (for sitewide values), or through the meta
data of each specific piece of content. You can define any values of any
type (supported by your front matter/config format) and use them however
you want to inside of your templates. 


## Using Content (page) Parameters 

In each piece of content you can provide variables to be used by the
templates. This happens in the [front matter](/content/front-matter). 

An example of this is used in this documentation site. Most of the pages
benefit from having the table of contents provided. Sometimes the TOC just
doesn't make a lot of sense. We've defined a variable in our front matter
of some pages to turn off the TOC from being displayed. 

Here is the example front matter:

```
---
title: "Permalinks"
date: "2013-11-18"
aliases:
  - "/doc/permalinks/"
groups: ["extras"]
groups_weight: 30
notoc: true
---
```

Here is the corresponding code inside of the template:

      {{ if not .Params.notoc }}
        <div id="toc" class="well col-md-4 col-sm-6">
        {{ .TableOfContents }}
        </div>
      {{ end }}



## Using Site (config) Parameters
In your top-level configuration file (eg, `config.yaml`) you can define site
parameters, which are values which will be available to you in chrome.

For instance, you might declare:

```yaml
params:
  CopyrightHTML: "Copyright &#xA9; 2013 John Doe. All Rights Reserved."
  TwitterUser: "spf13"
  SidebarRecentLimit: 5
```

Within a footer layout, you might then declare a `<footer>` which is only
provided if the `CopyrightHTML` parameter is provided, and if it is given,
you would declare it to be HTML-safe, so that the HTML entity is not escaped
again.  This would let you easily update just your top-level config file each
January 1st, instead of hunting through your templates.

```
{{if .Site.Params.CopyrightHTML}}<footer>
<div class="text-center">{{.Site.Params.CopyrightHTML | safeHtml}}</div>
</footer>{{end}}
```

An alternative way of writing the "if" and then referencing the same value
is to use "with" instead. With rebinds the context `.` within its scope,
and skips the block if the variable is absent:

```
{{with .Site.Params.TwitterUser}}<span class="twitter">
<a href="https://twitter.com/{{.}}" rel="author">
<img src="/images/twitter.png" width="48" height="48" title="Twitter: {{.}}"
 alt="Twitter"></a>
</span>{{end}}
```

Finally, if you want to pull "magic constants" out of your layouts, you can do
so, such as in this example:

```
<nav class="recent">
  <h1>Recent Posts</h1>
  <ul>{{range first .Site.Params.SidebarRecentLimit .Site.Recent}}
    <li><a href="{{.RelPermalink}}">{{.Title}}</a></li>
  {{end}}</ul>
</nav>
```


[go]: <http://golang.org/>
[gohtmltemplate]: <http://golang.org/pkg/html/template/>

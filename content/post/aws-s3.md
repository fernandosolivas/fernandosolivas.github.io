+++
title = "AWS Simple Storage Service (S3)"
description = "Nesse post, vamos cobrir as propriedades do serviço S3."
date = "2020-03-08"
hidden = true
categories = [
    "AWS",
    "solutions architect",
    "certifications",
    "s3"
]
+++

![cover](/static/s3/cover.png)

Considere esse post, minhas notas para a certificação de Solutions Architect Solutions da AWS.*

Iremos cobrir um dos serviços mais antigos da AWS, o Simple Storage Service (S3). S3 é um serviço object-storage, ou seja, 
permite que você gerencie arquivos, entre outras características.

Não iremos passar por todos os aspectos do S3 e suas configurações e sim pelos principais tópicos dos simulados da 
certificação.

*_Não considere esse post como fonte da verdade. Haverão erros (espero que o mínimo possível) pois serão notas que pretendo revisar durante todo processo. Feedbacks e correções serão super bem vindas na seção de comentários abaixo._

## Características base

- S3 é baseado em objetos (arquivos)
- Arquivos podem ser de 0 bytes a 5TB
- O armazenamento é ilimitado
- Arquivos são armazenado em Buckets*
- Bucket é um namespace universal, logo ele precisa ser único na AWS
- Ao realizar o upload de um arquivo, a resposta do S3 é um estado HTTP 200
- Disponibilidade garantida de 99,99%
- Garantia de durabilidade de 99,9999999999% (11 x 9s)
- Camada de armazenamento disponível (Tiered Storage)
- Gerenciamento de ciclo de vida
- Versionamento
- Encriptação
- MFA para deleção de recursos  
- Proteção de dados através de lista de controle de acesso (ACL) e Políticas de Bucket
- Namespace universal na AWS. Logo, o nome do Bucket deverá ser único em toda AWS. 

## Principais Termos
Antes de falarmos mais sobre S3, é necessário que a gente fale a mesma língua. Por isso, vamos passar por alguns termos 
e nomes do S3.

#### Buckets

Um bucket é similar a uma pasta. Vamos criar nosso primeiro Bucket.

No Console de Gerenciamento da AWS, vamos escolher o serviço S3.

![s3](/static/s3/select-s3.png)

Já na página do serviço, iremos clicar no botão "Criar bucket" e seguir com a criação do nosso Bucket que utilizaremos daqui pra frente.

![create-bucket](/static/s3/create-bucket.png)
_O nome do bucket será nossa url de acesso posteriormente. Esse é um dos motivos do nome ser universal. Então, escolha um nome
da sua preferência pois se tentar replicar o nome do bucket do arquivo alguém já estará utilizando._

Perceba que no footer deste modal existem 3 botões. Criaremos o bucket direto, sem rever políticas e outras opções (vamos passar por elas uma a uma).

Após a criação, você deve ver seu bucket criado*.

![create-bucket-2](/static/s3/create-bucket-2.png)

_Perceba que a coluna **Acesso** está com o valor: "Bucket e objetos não públicos". Assim como um novo usuário no IAM, todo
bucket novo é criado sem acesso público_.

#### Objetos

Objetos são arquivos e consite em: chave, valor, identificador de versão, metadados e subrecursos (Access Control List, Torrent). A chave é o nome do arquivo e o valor é o conteúdo, uma sequência de bytes.

Vamos criar nosso primeiro arquivo dentro do bucket criado no passo anterior. Vamos entrar dentro do bucket e selecionar 
**Carregar** para realizar upload de um arquivo para nosso bucket. Após concluir você deve ver seu arquivo disponível já no bucket.

Ao clicar no arquivo, abrirá um menu lateral com as opções de configuração deste arquivo.

![upload-s3](/static/s3/upload-s3.png)
 
Perceba que a url de acesso ao S3 é formada por: [protocolo_http]://[nome_do_bucket].s3.amazonaws.com/[nome_do_arquivo],
 no meu caso fica assim: https://fernandosoliva-aws-s3.s3.amazonaws.com/17.jpg
 
 Vamos tentar acessar essa URL?
 
 ![foribidden](/static/s3/read-file-s3.png)
 
 Lembra que falamos que todo bucket criado sem nenhuma configuração específica fica privado por padrão? Esse é um dos 
 motivos do acesso negado ao arquivo... Então, vamos trocar as configurações do nosso bucket. Dentro do seu bucket, 
 tem algumas abas como opção: _Visão geral_, _Propriedades_, _Permissões_, _Gerenciamento_ e _Pontos de acesso_. Nesse
 momento, vamos entrar na aba **Permissões**.
 
 ![public-access-config](/static/s3/public-access-config-1.png)
 
 Clique em **Editar** e desmarque a opção _Bloquear todo o acesso público_ por enquanto. Salve e acesse novamente seu arquivo.
 
 ![foribidden](/static/s3/read-file-s3.png)
 
 Acesso negado novamente?! O que aconteceu?! É que essa configuração habilita com que neste bucket **possa** ter objetos
 públicos **mas não que todos objetos serão públicos neste bucket**. Então, é necessário habilitar individualmente cada objeto.
 
 Clique novamente em seu arquivo, desta vez com o botão direito e selecione a opção *Tornar público*. Clique na url deste
 objeto e você deverá conseguir vê-lo sem problemas!
 
#### Consistência dos dados
Esse ponto é mais complicado realizar testes mas precisamos sempre lembrar das características da consistência de dados do S3, 
são elas:

- Leitura imediata depois de escrita para novos objetos
- Consistência eventual para sobrescrita e deleção de arquivos. Pode demorar um pouco para propagar a alteração realizada.

## Categorias de Armazenamento
Este é um tema bem popular dos simulados da certificação da AWS. Saber reconhecer quando utilizar cada um dos tipos
de armazenamento da AWS e lembre-se **elas são aplicáveis aos objetos e não aos buckets**.

Existem 7 tipos:

- Padrão
- Divisão Inteligente em Camadas
- Padrão - Acesso infrequente
- Uma zona - Acesso infrequente
- Glacier
- Glacier Deep Archive
- Redundância Reduzida

![chart](/static/s3/s3-chart.png)
_Retirado de https://aws.amazon.com/s3/storage-classes/ seção Performance Chart_

#### Padrão
Esse tipo é escolhido quando não escolhemos que categoria de armazenamento queremos para nosso arquivo, ele oferece alta
resiliência, disponibilidade e performance. É bom para dados que requerem acesso com frequência como os cenários de:
Distribuição de conteúdo, sites dinâmicos, dados analíticas e outras formas de dados que são acessados com frequência.

#### Divisão Inteligente em Camadas
A _Divisão Inteligente em Camadas_ é bem similar ao S3 Padrão, porém ela tem como foco otimizar os custos do seu objeto
armazenado, utilizando inteligência artificial/machine learning para identificar qual melhor _categoria de armazenamento_ seu
objeto deveria estar alocado.

Então este tipo herda todas as características do **Padrão** porém tem como foco otimização. Porém também é cobrado uma taxa
de monitoramento por objetos, então fique atento, **para grande escala de objetos, ele será mais caro que o padrão**.

#### Padrão - Acesso infrequente
Este categoria de armazenamento é indicado para dados acessados com menos frequência e que exigem acesso rápido quando necessário.
oferece os altos níveis de resiliência e throughput e a baixa latência da categoria *S3 Padrão* com taxas reduzidas por GB de armazenamento e GB de recuperação.

#### Uma zona - Acesso infrequente

Todas os tipos da AWS oferecem replicação do seu objeto por pelo menos três zonas de disponibilidade. Esse tipo oferece
somente uma zona com custo reduzido a 20% em comparação ao *Padrão - Acesso infrequente*, então tenha em mente que, 
se essa zona falhar você perderá seu arquivo. Um bom caso de uso para esse tipo é quando você pode perder o dado no S3 
que é fácil de reproduzí-lo na fonte de dados.

#### Glacier
O Glacier é bom para armazenar dados que não necessitam retorno imediato quando solicitado com baixo custo de armazenamento, 
além de oferecer altos níveis de resiliência aos seus dados com a estratégia de no mínimo três zonas de replicação de seus dados. 

#### Glacier Deep Archive

O S3 Glacier Deep Archive é a classe de armazenamento mais barata do Amazon S3 e oferece suporte à retenção e 
preservação digitais de longo prazo para dados que podem ser acessados uma ou duas vezes por ano. 

Essa classe é projetada para clientes que mantêm conjuntos de dados por 7 a 10 anos ou mais para cumprir requisitos de 
conformidade normativa, especialmente em setores altamente regulados como serviços financeiros, saúde e setores públicos. 

Todos os objetos armazenados no S3 Glacier Deep Archive são replicados e armazenados em, pelo menos, três zonas de disponibilidade 
distribuídas geograficamente, protegidos por 99,999999999% de resiliência e podem ser restaurados em até 12 horas._


#### Trocando o categoria de armazenamento

Para trocar o categoria de armazenamento é simples, clique em seu arquivo no S3 para um menu lateral aparecer (direita).

![change-category](/static/s3/change-storage-category.png)

Clique em **Propriedades** e em seguida clique em **Categoria de armazenamento** e você deve ver algo similar a imagem a seguir.

![price-chart](/static/s3/s3-storage-category.png)

#### Mais informações
A AWS possui uma [página específica](https://aws.amazon.com/pt/s3/storage-classes/) sobre as características de cada um desses _tipos de armazenamento_.

Como S3 é um dos serviços mais antigos da AWS e um dos mais utilizados, é também um dos tópicos mais populares da prova
de certificação. Nos simulados que apliquei, existiam MUITAS perguntas baseadas em cenários para escolher qual melhor dos 
_tipos de armazenamento_ para meu cenário, baseado em preço, disponibilidade e throughput. Minha dica: Aprenda bem sobre 
o S3 e seus _tipos de armazenamento_.

## Segurança e Encriptação

Como mencionado anteriormente, os buckets criados são por padrão **PRIVADOS** e você pode mudar o acesso a eles através de **Lista de Controle de Acesso**(ACL) e/ou **Políticas**.

É possível também possível configurar logs de acesso ao Bucket. Esses logs podem ser mantidos na sua conta ou enviar para outras contas.

#### Lista de controle de acesso (ACL)
ACLs são aplicáveis ao bucket, então é um ponto de atenção ao liberar permissões.

No menu de configurações do seu bucket, na aba **Permissões** é possível encontrar algumas opções de permissionamento de 
acesso ao seu bucket, clique em _Lista de Controle de Acesso_ para configurar uma nova lista.

![acl](/static/s3/acl.png)

Nessa imagem vemos que existem alguns tipos de configurações de ACL, uma que é fortemente aconselhada a **NÃO** se utilizar,
é a seção **Acesso público** para todos, mesmo que seja de listagem. A não ser que você tenha um bom motivo e garantia de 
um ambiente controlado de permissão, é bastante aconselhável a não se utilizar este meio.

ACLs são ótimas para dar acesso a contas de terceiros a manusear seus objetos. 

Um exemplo frequente é: **Você faz parte de uma consultoria e precisa acessar os objetos de seu cliente no S3. 
Seu seu cliente não vê necessidade de criar um usuário somente para isso acesso ao S3, então você propõe que sua conta 
da AWS seja adicionada a ACL do bucket que você precisa acesso, conseguindo assim acessar o recurso necessário 
sem comprometer a segurança de outros serviços de seu cliente.**

#### Encriptação
A encriptação dos dados do S3 é feita de duas formas:

- **Em trânsito**: Encriptação realizada através de SSL/TLS quando um arquivo é solicitado através de requisições HTTPS. 
- **Encriptação no Servidor**: Essa encriptação é feita do lado do servidor através de chaves providas pelo cliente ou gerenciadas pela AWS.

Nesse tópico, falaremos somente da **Encriptação no Servidor**. É possível realizá-la com os seguintes métodos de encriptação.

- Chaves Gerenciadas pelo S3 - SSE-S3
- AWS Key Management Service (KMS)* - SSE-KMS
- Encriptação no lado do servidor com senha provida pelo cliente - SSE-C
*KMS é um tema tratado na certificação de Segurança da AWS e por isso não utilizaremos ele neste momento.


## Encriptando seus dados via S3
Agora vamos voltar ao menu de propriedades do nosso arquivo. Uma das opções que vemos é **Criptografia**. Vamos clicar nela
para configurar nosso modo de criptografia e selecione **AES-256** e confirme.

![criptografia](/static/s3/criptografia.png)

Veja que só existem 3 opções: _Nenhum_, _AES-256_ e _AWS-KMS_. 

- **Nenhum**: é auto explicativo.
- **AES-256**: Chave e criptografia gerenciada totalmente pela AWS.
- **AWS-KMS**: Criptografia através de chave criada pelo serviço KMS. Como dito antes, não iremos entrar em detalhes deste meio,
mas é importante saber que o mesmo existe.

A criptografia é transparente na leitura e escrita de dados via API, SDK e CLI. 
**Essa encriptação ocorre no arquivo salvo em disco e antes da leitura a AWS decriptografa e retorna o dado cru para visualização. 
Ou seja, tem como objetivo proteger caso ocorra algum vazamento ou falha de segurança dos discos físicos que seus dados 
sejam lidos sem a chave original de criptografia.**

Lembre-se:

- A encriptação pode ocorrer em trânsito(SSL/TLS) ou do lado do servidor.
- A encriptação do lado do servidor é necessário habilitar.
- Esta encriptação pode ser habilitada a nível de bucket ou de arquivos.

#### Versionamento
O versionamento é uma ferramenta essencial em um mundo de mudanças constantes e ágeis. Vamos realizar a configuração do 
versionamento de nosso bucket.

Na aba _Propriedades_ do seu bucket tem uma opção chamada _Controle de Versões_. É possível habilitar o controle de versões 
selecionando a opção como a imagem a seguir.

![permissions](/static/s3/permissions.png) 

O versionamento passará a funcionar para novos objetos, então vamos enviar uma nova versão do mesmo arquivo que vinhamos 
editando até o momento e habilite a visualização de versões no seu bucket.

![versioning](/static/s3/versioning.png)

Você deve ter o comportament similar ao visto na imagem anterior. Uma versão de seu arquivo está **null** e outra com um 
identificador auto gerado. A versão **null** é o arquivo antes da habilitação do versionamento.

Vamos deletar nosso arquivo para ver o comportamento que o versionamento realiza. Vamos ocultar as informações de versões 
e apagar nosso arquivo.

![excluding](/static/s3/excluding.png)

Habilite novamente a visualização de versões.

![excluded-version](/static/s3/excluded-version.png)

Veja que uma versão com uma descrição **Marcador de exclusão** foi gerada. Esse marcador é para identificar seu arquivo excluído 
e a versão do mesmo, facilitando uma recuperação rápida. Para recuperar nosso arquivo apagado, basta apagar a versão 
com a descrição **Marcador de exclusão**.

Veja que ao excluir esta versão, seu arquivo estará restaurado no bucket.
  
Revendo funções de versionamento:

- Guarda todas as versões de alteração do arquivo, incluindo deleções.
- Uma vez habilitado, não pode ser desabilitado*. Você precisa excluir e criar um novo Bucket.
- Integração com Regras de Ciclo de Vida do S3.
- Possibilidade de adicionar MFA para deleção de arquivos como mais uma camada de segurança.

*É possível suspender o versionamento, porém não desabilitar permanentemente e perder as versões anteriores.

#### Gerenciamento de Ciclo de Vida

Com o Gerenciamento de Ciclo de Vida é possível transferir seus arquivos entre as categorias de armazenamento de forma 
automatizada conforme critérios que você define. Esta configuração é a nível de bucket.

Quando o versionamento está habilitado é possível realizar regras em versões passadas.

## Configurando o Ciclo de Vida
Para configura o ciclo de vida de seu bucket, selecione a aba **Gerenciamento** e a opção **Ciclo de Vida**. Clique em
_Adicionar regra de ciclo de vida_.

A criação de ciclo de vida é bastante flexível e você pode criar regras de ciclo de vida para prefixo e tags específicas 
ou para todos os objetos. Para nosso exemplo, iremos criar a regra para todos os objetos.

![lifecicle-1](/static/s3/lifecicle-1.png)

Agora vem as configurações para transição das categorias de armazenamento. No meu caso eu optei em transicionar as versões 
atuais com mais de 10 dias para a categoria de _Divisão Inteligente de Armazenamento_. Já para as versões anteriores 
selecionei a transição para a categoria **Acesso Infrequente** após 10 dias como "versão anterior".

![lifecicle-2](/static/s3/lifecicle-2.png)

É possível também realizar expiração de versões. Irei selecionar a exclusão de versões anteriores após 395 dias e também 
a exclusão de versões que não houve conclusão de upload e/ou apagar marcadores de exclusão expirados.
 
![lifecicle-3](/static/s3/lifecicle-3.png)

Salve seu ciclo de vida para efetivar a configuração.
![lifecicle-4](/static/s3/lifecicle-4.png)

Lembre-se:

- Regras de ciclo de vida são a nível de bucket.
- Ciclo de vida tem como objetivo automatizar a transição de objetos entre categorias de armazenamento e/ou exclusão de 
dados não mais necessários.

#### Compartilhamento de Bucket entre Contas

Existem 3 formas de compartilhar Buckets através de contas:

- Utilizando Políticas de Bucket & IAM (esse meio é aplicável a todo Bucket). Só é possível fazer através de *Acesso Programático*
- Utilizando Lista de Controle de Acesso do Bucket & IAM (esse meio é aplicável a objetos individuais). Só é possível fazer através de *Acesso Programático*
- E Funções IAM Entre Contas. É possível fazer através de *Acesso Progrmático* e *Console de Gerenciamento AWS*

#### Replicação entre Regiões

Para habilitação dessa funcionalidade é necessário habilitar o *Versionamento* tanto no bucket de origem quanto no de destino. 
Arquivos já existentes antes da ativação da funcionalidade, não serão replicados entre as regiões, somente novos arquivos.

Vamos criar um novo bucket para utilizar como réplica de nosso bucket inicial. Este bucket precisa estar em uma região diferente. 
No meu caso irei criar um bucket chamado **fernandosoliva-aws-s3-replication** na região de São Paulo, assim fica fácil de lembrar e achar.

Entrarei no painel de configuração de meu bucket inicial **fernandosoliva-aws-s3** e na aba **Gerenciamento** selecionarei 
a opção **Replicação** e clicar em _Adicionar Regra_.

![replication-1](/static/s3/replication-1.png)

Selecione nosso bucket de destino

![replication-2](/static/s3/replication-2.png)

Como não habilitamos versionamento a AWS irá pedir para ligar esta funcionalidade no bucket de destino

![replication-3](/static/s3/replication-3.png)

Dê um nome e selecione uma [Função IAM](https://fernandosoliva.com/post/aws-iam) ou crie uma

![replication-4](/static/s3/replication-4.png)

E revise sua replicação.

![replication-5](/static/s3/replication-5.png)

Agora que temos nossa replicação funcionando, vamos adicionar mais um arquivo em nosso bucket e ver o comportamento de 
replica acontecer.

Agora que vimos nossa replica funcionando, que tal deletarmos um arquivo?! O que será que deveria acontecer?!

Nosso arquivo não é deletado. É uma "trava" de proteção da AWS para que deleções erradas não sejam replicadas.


Relembrando alguns detalhes:

- O conteúdo de um bucket é replicado para uma outra região. Não é possível replicar para duas ou mais regiões.
- As regiões devem ser diferentes. Você não pode replicar da região de São Paulo para ela mesma.
- A replicação é feita a nível de arquivos.
- A marca de deleção *não* é replicada. Caso queira que o arquivo seja deletado das duas regiões, é preciso deletá-lo em ambas as regiões.


## Conclusão

Tem muita coisa ainda sobre S3, como: metadados, tags, bloqueio de objetos, CORS, distribuição de sites e outros. Porém 
a distribuição de sites é um dos temas que vou tratar em outro artigo sobre o CloudFront. Apesar de ser muito utilizado 
com o S3 e ser um tópico que cai com frequência nos simulados, preferi fazer um outro artigo sobre o serviço para não 
criar um artigo maior do que este já está.

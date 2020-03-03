+++
title = "AWS Identity and Access Management"
description = "Criação e gerenciamento de usuários, grupos, políticas e funções"
date = "2020-03-02"
hidden = true
categories = [
    "AWS",
    "solutions architect",
    "certifications",
    "iam"
]
+++

Considere esse post, minhas notas para a certificação de Solutions Architect Solutions da AWS.*

São notas e explicações que espero me ajudar e a outras pessoas também enquanto estiver nesse processo! 
A princípio serão posts dos principais serviços utilizados na prova da certificação, então iremos começar pelo IAM.

_Se você não está familiarizado com AWS, esses posts podem te deixar um pouco confuso. Aconselho primeiramente acessar o curso gratuito da [AWS Cloud Practictioner](https://aws.amazon.com/pt/training/course-descriptions/cloud-practitioner-essentials/)_

Para realizar qualquer passo de criação de acesso a serviços, você primeiramente terá que criar uma conta na AWS e acessar o console de gerenciamento.

*_Não considere esse post como fonte da verdade. Haverão erros (espero que o mínimo possível) pois serão notas que pretendo revisar durante todo processo. Aceito feedbacks e correções através do email me@fernandosoliva.com_
## Identity and Access Management (IAM)

O primeiro serviço de estudo é o [Identity and Access Management (IAM)](https://aws.amazon.com/pt/iam/). O IAM é um serviço de gerenciamento de acesso da sua conta AWS,
permitindo com que você controle de forma mais granular todo os acessos feitos a sua conta, seja um usuário da sua conta, um terceiro ou uma aplicação.


#### Principais Características
- Permissões granulares
- Federação de Identidade (Active Directory, Facebook, LinkedIn, etc.)
- Autenticação Multi Fator (MFA)
- Controle centralizado da sua conta AWS
- Acesso compartilhado da sua conta AWs
- Regras de rotação de senhas
- Integração com muitos serviços da AWS
- [PCI DSS Complience](https://www.pluralsight.com/paths/payment-card-industry-data-security-standard-pci-dss)

#### Entendendo as entidades

O IAM possui 4 entidades de gerenciamento, são elas:
- Usuários
- Grupos
- Funcões
- Políticas

#### Usuário
Usuário final. Geralmente pessoas que acessam sua conta e serviços da AWS.

#### Grupos
Coletivo de usuários. Todos usuários do grupo irão herdar as permissões daquele grupo.

#### Funcões
Permite com que você delegue acesso a usuários ou serviços permitindo geração de credenciais temporárias para que você não precise compartilhar credenciais de longo prazo.

#### Políticas
Documentos que especificam permissões de um ou mais serviços. São em formato JSON e podem ser adicionados usuários, roles e grupos.

e.g
```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "cognito-identity:Describe*",
                "cognito-identity:Get*"
            ],
            "Resource": "*"
        }
    ]
}
```

## Usuários
Como dito anteriormente, usuários são pessoas / aplicações que irão acessar seus recursos na AWS. Vamos criar nosso primeiro usuário! Primeiro vamos acessar o serviço IAM na AWS.

![iam-service-console](/IAM.png)

Dentro do console iremos selecionar *Usuários no menu lateral esquerdo* e em *Criar usuário*, até chegar na imagem abaixo.

![create-user-1](/create-user-1.png)

Observações sobre os items na imagem, acredito que todos os campos são alto explicativos porém vamos entrar em detalhes em *acesso programático* e *acesso ao console de gerenciamento da AWS*. Ambas as configurações são regováveis posteriormente.

Seguindo a criação de usuário, vamos associar permissões ao usuário através das *políticas*. Lembre-se que *todo usuário criado, não contém nenhum permissão por padrão*.

Existem três formas de se adicionar políticas ao usuário através de: Grupos, Cópia de um usuário já existente ou adicionar diretamente cada política no usuário.

Nesse momento iremos fazer a terceira opção, *adicionar diretamente ao usuário*. Selecione a terceira opção que aparecem na segunda etapa da criação do usuário e associe uma permissão. Para esse caso, selecione *AdministratorAccess*.

![attach-policy](/create-user-2.png)

Clique em *Próximo: Tags* e associe uma tag de identificação. Tags são importantes para destacar e segmentar recursos. Futuramente, podemos criar alarmes por alguns fatores, um deles são as tags.

![attach-tag](/create-user-3.png)

Clicando em *Próximo: Revisar* será possível ver todas as características do usuário que estamos criando. Clique em próximo e pronto, seu usuário foi criado!

![success](/create-user-5.png)

Nesta última etapa a AWS te da algumas informações do usuário, como chave de acesso via CLI e/ou aplicações (Access Key e Secret Key), usuário e senha para acesso através do console e um link com instruções de acesso. Todas essas informações podem ser baixadas através de um CSV, clicando no botão *Fazer download .csv*

*ATENÇÃO:* As credenciais são imutáveis e senhas só podem serem lidas nesse momento, sendo assim, é importante que você baixe essas credenciais e faça um backup seguro desses dados. Caso mesmo assim você perca este backup é possível gerar novas chaves de acesso através do painel.

Busque o usuário que criamos e clique nele para editar.
![cred-1](/cred.png)

Vá até a aba *Credenciais de segurança* e no seção *Chaves de acesso* você pode tornar inativa a chave já existe e/ou criar uma outra. Lembre-se: Essas chaves de acesso são para utilização da AWS CLI e/ou aplicações.

![cred-2](/cred-2.png)

Para redefinir senha, clique em _Gerenciar_ na opção *Senha de console*.

#### Acesso Programático
São credenciais de acesso para utilização da AWS CLI e/ou aplicações. É possível ter duas credenciais ativas por vez e sempre é possível expirar credenciais já existentes.

#### Acesso ao console de gerenciamento da AWS
Diferente das credenciais de *Acesso Programático* essas informações servem para acesso através do console de gerenciamento da AWS. É possível forçar que o usuário altera a senha a qualquer momento.

*As credenciais de _Acesso Programático_ e _Acesso ao console de gerenciamento da AWS_ são informações diferentes e elas não se substituem, logo, não é possível utilizar _Access Key_ e _Secret Key_ para logar via console e nem _email_ e _senha_ para utilizar em CLI e aplicações.


## Políticas

É possível também criar políticas customizadas. Vamos supor que eu queria que um usuário tenha somente acesso de leitura em um bucket chamado *road-to-saa* do [S3](https://aws.amazon.com/s3/) e escrita em todos os tópicos do [SNS](https://aws.amazon.com/sns).

Vamos entrar no painel do IAM novamente e no menu lateral selecionar políticas. Você verá uma lista de todas as políticas já existentes na sua conta. Existem três tipos de políticas.

- Cliente Gerenciado: Políticas customizadas criadas por usuários da AWS.
- Funções de Trabalho: Políticas pré definidas pela AWS que exemplificam funções de trabalho. Existem atualmente dez políticas pré definidas pela AWS.
- Gerenciado pela AWS: Assim como funçoes de trabalho, são políticas gerenciadas pela AWS e elas exemplificam caso de usos de serviços. e.g: _AmazonS3ReadOnlyAccess: Provê acesso de leitura a todos os buckets da AWS via *Console de Gerenciamento*_

Agora criaremos duas políticas, uma chamada _RoadToSAA-Bucket-ReadOnly_ e outra _WriteAllTopics-SNS_.

Clique em _Criar Política_
![create-policy-1](/create-policy-1.png)

Escolha o *S3* na opção de serviço, selecione o checkbox _Leitura_ da seção de _Ações_, em _Recursos_ selecione bucket e clique em _Adicionar ARN_ e digite o nome do nosso bucket *road-to-ssa* *
![create-policy-2](/create-policy-2.png)

Clique em _Revisar Política_ e nesse momento e onde daremos o nome da nossa política.

![create-policy-3](/create-policy-3.png)

Pronto, criamos a nossa primeira política. Iremos repetir os mesmos passos, porém escolheremos o serviço SNS e marcaremos o checkbox _Gravação_.

Temos nossas duas políticas criadas e agora iremos associar ao nosso usuário criado anteriormente.

Agora no Menu do IAM, selecione a opção *Usuários* no menu lateral, selecione o usuário criado anteriormente e na aba _Permissões_ clique no botão _Adicionar permissões_. Iremos parar nessa página e selecionar a terceira opção: _Anexar políticas existentes de forma direta_.

Clique em _Filtrar políticas_ e selecione _Cliente gerenciado_. Irá retonar todas as políticas criadas por usuários e não gerenciadas pela AWS. Selecione as duas _RoadToSAA-Bucket-ReadOnly_ e _WriteAllTopics-SNS_ e depois avance até o momento final de adição, clicando nos botões azuis.

Pronto! Temos nosso usuário com permissões limitadas de acesso em nosso bucket e no SNS. O mesmo pode ser feito para todos os outros recursos da AWS e usuários!

## Grupos
Com grupos, conseguimos contextualizar usuários em grupos de acesso. Por exemplo, adicionar os gerentes da sua empresa como administradores para que eles possam ter autonomia de gerenciar seus respectivos times, ou, criar um grupo de DevOps onde eles terão acesso ilimitado a stack da sua empresa, podendo desenvolver e operacionar sua infra de forma ágil. Podemos usar nossa imaginação e pensar em vários casos onde a divisão faça sentido.

Assim como usuários, é possível definir políticas aos grupos e, obviamente, usuários.

No menu lateral clique na opção _Grupos_ e em seguida _Criar novo grupo_. Escolha um nome para este grupo, no meu caso se chamará *developers* e em seguida as políticas desses usuários. No meu caso, irei associar _AWSCodePipelineFullAccess_, _AWSCodeCommitFullAccess_ e _AWSCodeBuildDeveloperAccess_.

Clique em _Próxima etapa_ no botão inferior e na você verá uma tela de revisão similar a esta. 

![create-policy-4](/create-policy-4.png)

Por fim clique em _Criar grupo_. Agora você será redirecionado para a home do menu de grupos. Selecione seu grupo criado e você verá uma tela e uma mensagem em destaque *Este grupo não contém nenhum usuário*. Clique no botão _Adicionar usuários ao grupo_ e selecione o usuário anteriormente criado.

![create-group](/create-group.png)

Pode ver que nessa mesma área, você consegue adicionar mais usuários ou remover usuários já existente nesses grupos.

## Funções
Como dito anteriormente, você pode configurar funções para que terceiros tenham acesso a sua conta da AWS e a seus recursos.

Existem 4 tipos de entidades de funções:

- *Serviço da AWS*: _Dá permissão a um serviço executar ações em seu nome. e.g: Instância EC2 escreva em um bucket do S3._
- *Outra conta AWS*: _Dá permissão a uma outra conta executar ações na sua conta. e.g: Uma prestadora de serviço conectar a um banco do RDS de fora da sua conta._
- *Identidade da web*: _Permite que os usuários federados pela identidade da Web externa especificada ou pelo provedor OpenID Connect (OIDC) assumam essa função para executar ações em sua conta_
- *Federação do SAML 2.0*: _Permite que os usuários que são federados com o SAML 2.0 assumam essa função para executar ações em sua conta_

O processo de adicionar políticas a funções é bem similar ao de adicionar à usuários, por isso não irei repetir os passos para ficarmos menos repetitivos.

## Conclusão

- IAM é global, ou seja, não tem região específica e um usuário, função, grupo ou política criado no IAM poderá acessar todas as regiões da AWS.
- A conta "root" é a conta que foi criada a primeira vez. Essa conta tera acesso de Administrador completo (o famoso _God Mode_).
- Usuários não tem permissões quando criados.
- Usuários recem criados deverão ter acesso de console ou acesso programático.
- Access Key ID e Secret Access e Usuário e senha não são a mesma coisa. Não é possível utilizar Acess Key e Secret Access para acessar via console, assim como não é possível usar email e senha para realizar acesso programático.
- Sempre crie MFA na sua conta root. (Lembre de realizar um backup seguro do seu QRCode).
- Customize rotações de senhas e critérios para criação de senhas.
- Utilize sempre controle fino das permissões. Seja menos permissivo sempre!

+++
title = "AWS Simple Storage Service (S3) (WIP)"
description = "Nesse post, cobrir as propriedades do serviço S3."
date = "2020-03-06"
hidden = true
categories = [
    "AWS",
    "solutions architect",
    "certifications",
    "s3"
]
+++

Considere esse post, minhas notas para a certificação de Solutions Architect Solutions da AWS.*

Iremos cobrir um dos serviços mais antigos da AWS, o Simple Storage Service (S3). S3 é um serviço object-storage, ou seja, permite que você gerencie arquivos, entre outras características.

*_Não considere esse post como fonte da verdade. Haverão erros (espero que o mínimo possível) pois serão notas que pretendo revisar durante todo processo. Aceito feedbacks e correções através do email me@fernandosoliva.com_

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
- Proteção de dados através de lista de controle de acessos (ACL) e Políticas de Bucket

#### Objetos

Objetos são arquivos e consite em: chave, valor, identificador de versão, metadados e subrecursos (Access Control List, Torrent). A chave é o nome do arquivo e o valor é o conteúdo, uma sequência de bytes.

#### Consistência dos dados

- Leitura imediata depois de escrita para novos objetos
- Consistência eventual para sobrescrita e deleção de arquivos. Pode demorar um pouco para propagar a alteração realizada.

## [Tipos de armazenamento](https://aws.amazon.com/s3/storage-classes/)

- Padrão
- Divisão Inteligente em Camadas
- Padrão - Acesso infrequente
- Uma zona - Acesso infrequente
- Glacier
- Glacier Deep Archive
- Redundância Reduzida

Abaixo as características e preços de cada categoria do S3

![price-chart](/s3-storage-category.png)
![chart](/s3-chart.png)
_Retirado de https://aws.amazon.com/s3/storage-classes/ seção Performance Chart_

## Padrão

"_O S3 Standard oferece um armazenamento de objetos com altos níveis de resiliência, disponibilidade e performance para dados acessados com frequência. Como fornece baixa latência e alto throughput, o S3 Standard é adequado para uma grande variedade de casos de uso, como aplicativos na nuvem, sites dinâmicos, distribuição de conteúdo, aplicativos móveis e de jogos e dados analíticos de big data. As classes de armazenamento S3 podem ser configuradas no nível do objeto, e um único bucket pode conter objetos armazenados no S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA e no S3 One Zone-IA. Você também pode usar as políticas de ciclo de vida do S3 para migrar automaticamente objetos entre classes de armazenamento sem nenhuma alteração nos aplicativos._" - Documentação AWS S3, seção [*S3 Standard*](https://aws.amazon.com/pt/s3/storage-classes/)

## Divisão Inteligente em Camadas

"_A classe de armazenamento S3 Intelligent-Tiering foi projetada para otimizar os custos movendo automaticamente os dados para o nível de acesso mais econômico, sem impacto na performance ou sobrecarga operacional. Ela funciona a partir do armazenamento de objetos em dois níveis de acesso: um nível é otimizado para acesso frequente e outro nível de custo mais baixo, que é otimizado para acesso infrequente. Por uma pequena taxa mensal de automação e monitoramento por objeto, o Amazon S3 monitora os padrões de acesso dos objetos no S3 Intelligent-Tiering e move aqueles que não foram acessados por 30 dias consecutivos para o nível de acesso infrequente. Se um objeto no nível de acesso infrequente for acessado, ele será automaticamente movido de volta para o nível de acesso frequente. Não há taxas de recuperação ao usar a classe de armazenamento S3 Intelligent-Tiering e não há taxas de níveis adicionais quando os objetos são movidos entre níveis de acesso. É a classe de armazenamento ideal para dados duradouros com padrões de acesso desconhecidos ou imprevisíveis. As classes de armazenamento S3 podem ser configuradas no nível do objeto, e um único bucket pode conter objetos armazenados no S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA e no S3 One Zone-IA. Você pode fazer upload de objetos diretamente ao S3 Intelligent-Tiering ou usar as políticas de ciclo de vida do S3 para transferir objetos do S3 Standard e do S3 Standard-IA para o S3 Intelligent-Tiering. Também é possível arquivar objetos do S3 Intelligent-Tiering no S3 Glacier._" - Documentação AWS S3, seção [*S3 Intelligent-Tiering*](https://aws.amazon.com/pt/s3/storage-classes/)

## Padrão - Acesso infrequente

"_O S3 Standard-IA é indicado para dados acessados com menos frequência, mas que exigem acesso rápido quando necessários. A categoria S3 Standard – IA oferece os altos níveis de resiliência e throughput e a baixa latência da categoria S3 Standard, com taxas reduzidas por GB de armazenamento e GB de recuperação. A combinação de baixo custo e alta performance tornam a classe S3 Standard-IA ideal para armazenamento de longa duração, backups e datastores para arquivos de recuperação de desastres. As classes de armazenamento S3 podem ser configuradas no nível do objeto, e um único bucket pode conter objetos armazenados no S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA e no S3 One Zone-IA. Você também pode usar as políticas de ciclo de vida do S3 para migrar automaticamente objetos entre classes de armazenamento sem nenhuma alteração nos aplicativos._" - Documentação AWS S3, seção [*Acesso inefrequente*](https://aws.amazon.com/pt/s3/storage-classes/)

## Uma zona - Acesso infrequente
"_O S3 One Zone-IA é indicado para dados acessados com menos frequência, mas que exigem acesso rápido quando necessários. Ao contrário de outras classes de armazenamento do S3, que armazenam dados em no mínimo três Zonas de disponibilidade (AZs), a S3 One Zone-IA armazena dados em uma única AZ, com um custo 20% inferior ao S3 Standard-IA. A classe S3 One Zone-IA é ideal para clientes que querem uma opção de menor custo para dados acessados com pouca frequência, mas não precisam da disponibilidade e da resiliência S3 Standard ou S3 Standard-IA. É uma ótima opção para armazenar cópias de backup secundária de dados locais ou que possam ser recriados com facilidade. Você também pode usar essa opção como um armazenamento barato para dados replicados de outra Região da AWS usando o replicador entre regiões do S3._

_A classe S3 OneZone-IA oferece os mesmos altos níveis de resiliência† e throughput e a baixa latência da classe S3 Standard, com taxas reduzidas por GB de armazenamento e GB de recuperação. As classes de armazenamento S3 podem ser configuradas no nível do objeto, e um único bucket pode conter objetos armazenados no S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA e no S3 One Zone-IA. Você também pode usar as políticas de ciclo de vida do S3 para migrar automaticamente objetos entre classes de armazenamento sem nenhuma alteração nos aplicativos._" - Documentação AWS S3, seção [*S3 One Zone-IA*](https://aws.amazon.com/pt/s3/storage-classes/)

## Glacier
_O S3 Glacier é uma classe de armazenamento segura, durável e de baixo custo para arquivamento de dados. Você pode armazenar com confiabilidade qualquer volume de dados a um custo competitivo ou inferior ao custo de soluções no local. Para manter os custos baixos, mas com condições de suprir necessidades variáveis, o S3 Glacier disponibiliza três opções de recuperação, que podem levar de alguns minutos a várias horas. Você pode fazer upload de objetos diretamente ao S3 Glacier ou usar as políticas de ciclo de vida do S3 para transferir dados entre qualquer uma classe de armazenamento do S3 para dados ativos (S3 Standard, S3 Intelligent-Tiering, S3 Standard-IA e S3 One Zone-IA) e S3 Glacier. Para obter mais informações, [acesse a página do Amazon S3 Glacier.](https://aws.amazon.com/glacier/)_ - Documentação AWS S3, seção [*Glacier*](https://aws.amazon.com/pt/s3/storage-classes/)


## Glacier Deep Archive

_O S3 Glacier Deep Archive é a classe de armazenamento mais barata do Amazon S3 e oferece suporte à retenção e preservação digitais de longo prazo para dados que podem ser acessados uma ou duas vezes por ano. Essa classe é projetada para clientes que mantêm conjuntos de dados por 7 a 10 anos ou mais para cumprir requisitos de conformidade normativa, especialmente em setores altamente regulados como serviços financeiros, saúde e setores públicos. O S3 Glacier Deep Archive também pode ser usado para casos de uso de backup e recuperação de desastres, além de ser uma alternativa mais barata e fácil de gerenciar em comparação aos sistemas de fita magnética como bibliotecas locais ou serviços externos. O S3 Glacier Deep Archive complementa o Amazon S3 Glacier, o que é ideal para arquivamentos em que os dados são recuperados com frequência e alguns desses dados precisam estar disponíveis em minutos. Todos os objetos armazenados no S3 Glacier Deep Archive são replicados e armazenados em, pelo menos, três zonas de disponibilidade distribuídas geograficamente, protegidos por 99,999999999% de resiliência e podem ser restaurados em até 12 horas._ - Documentação AWS S3, seção [*Glacier Deep Archive*](https://aws.amazon.com/pt/s3/storage-classes/)


#### Segurança e Encriptação

Como mencionado anteriormente, os Buckets criados são por padrão *PRIVADOS* e você pode mudar o acesso a eles através de *Lista de Controle de Acesso*(ACL) e/ou *Políticas*.

É possível também possível configurar logs de acesso ao Bucket. Esses logs podem ser mantidos na sua conta ou enviar para outras contas.

## Encriptação
Encriptação no S3 é feita ou do lado do servidor ou do lado do cliente. Em caso de encriptação do lado do cliente, ao realizar o upload de um arquivo, é necessário encriptar o conteúdo antes de realizar o envio do arquivo para o S3. No caso de encriptação no lado do servidor, a AWS irá encriptar o conteúdo do arquivo antes de salvá-lo no S3 e deixá-lo disponível para acesso e a encriptação é realizar com uma das três opções de chave de encriptação.

- Chaves Gerenciadas pelo S3 - *SSE-S3*
- AWS Key Management Service (KMS)* - *SSE-KMS*
- Encriptação no lado do servidor com senha provida pelo cliente - *SSE-C*
*KMS é um tema tratado na certificação de Segurança da AWS e por isso não utilizaremos ele neste momento.

(EXERCICIOS)

#### Versionamento
O versionamento no S3 tem as seguintes características:

- Guarda todas as versões de alteração do arquivo, incluindo todas as escritas e inclusive deleções.
- Uma vez habilitado, não pode ser desabilitado*. Você precisa excluir e criar um novo Bucket.
- Integração com Regras de Ciclo de Vida do S3.
- Possibilidade de adicionar MFA para deleção de arquivos como mais uma camada de segurança.

*É possível suspender o versionamento, porém não desabilitar permanentemente.

#### Gerenciamento de Ciclo de Vida

Essa funcionalidade do S3 tem como objetivo transferir arquivos do seu Bucket para outras camadas do serviço (Padrão, Divisão Inteligente em Cadas, ...) de forma automatizada conforme critérios que você define.

Não é obrigatório ter o *Versionamento* habilitado, porém com o versionamento habilitado é possível realizar ações em versões anteriores também.

#### Compartilhamento de Bucket entre Contas

Existem 3 formas de compartilhar Buckets através de contas:

- Utilizando Políticas de Bucket & IAM (esse meio é aplicável a todo Bucket). Só é possível fazer através de *Acesso Programático*
- Utilizando Lista de Controle de Acesso do Bucket & IAM (esse meio é aplicável a objetos individuais). Só é possível fazer através de *Acesso Programático*
- E Funções IAM Entre Contas. É possível fazer através de *Acesso Progrmático* e *Console de Gerenciamento AWS*

#### Replicação entre Regiões

Para habilitação dessa funcionalidade é necessário habilitar o *Versionamento* tanto no bucket de origem quanto no de destino. Arquivos já existentes antes da ativação da funcionalidade, não serão replicados entre as regiões, somente novos arquivos.

Essa funcionalidade é utilizável quando você quer que regiões diferentes tenham acesso ao mesmo conteúdo com baixa latência. Porém só é possível replicar um Bucket para uma região. Algumas características dessa funcionalidade.

- O conteúdo de um bucket é replicado para uma outra região. Não é possível replicar para duas ou mais regiões.
- As regiões devem ser diferentes. Você não pode replicar da região de São Paulo para ela mesma.
- A replicação é feita a nível de arquivos.
- A marca de deleção *não* é replicada. Caso queira que o arquivo seja deletado das duas regiões, é preciso deletá-lo em ambas as regiões.

#### CloudFront

Cloud Front é uma ferramenta poderosa de CDN (Content Delivery Network). Uma CDN são servidores distribuídos geograficamente que entregam o conteúdo para o usuário baseado na posição geográfica, da origem do conteúdo e do servidor de conteúdo.

Algumas características e funcionalidades:

- *Edge Location* - Servidor onde o arquivo é cacheado. Não só é possível ler um arquivo como também escrever. *Edge Location* não são AZ/Regiões.
- *Origin* - A origem de onde o arquivo será distribuído. Pode ser um Bucket do S3, uma instância de EC2, um LoadBalancer, um Route53 e outras possibilidades.
- *Distribution* - É a abstração da CDN, ou seja, uma agrupamento de todas as _Edge Location_. Existem dois tipos de Distribution: Web Distribuition (Websites) e RTMP (Streaming de Midia).
- Arquivos são cacheados até o [TTL](https://pt.wikipedia.org/wiki/Time_to_Live) expirar.
- É possível invalidar o cache do Edge Location, porém você será cobrado por esta ação.

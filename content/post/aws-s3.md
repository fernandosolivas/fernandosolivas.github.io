+++
title = "Entendendo AWS - S3"
description = "Nesse post, cobrir as propriedades do serviço S3."
date = "2020-03-02"
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

- Standard
- Intelligent-Tiering
- Standard-IA
- One Zone-IA
- Glacier
- Glacier Deep Archive

![chart](/s3-chart.png)
_Retirado de https://aws.amazon.com/s3/storage-classes/ seção Performance Chart_

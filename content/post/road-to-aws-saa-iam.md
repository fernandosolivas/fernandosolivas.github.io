+++
title = "Caminho para certificação AWS Solutions Architect Associate - Pt.1"
description = "Vamos aprender conceitos e realizar exercícios práticos de fixação."
date = "2020-02-29"
hidden = true
categories = [
    "AWS",
    "solutions architect",
    "certifications"
]
+++

Esse é o primeiro post da trilha de estudo que irei seguir até meu certificado da AWS de Solutions Architect Associate. São notas e explicações que espero me ajudar até o momento da prova e espero que ajude outras pessoas também!

#### Identity and Access Management (IAM)

O primeiro serviço de estudo é o Identity and Access Management (IAM). Este serviço prove interface para administração de: usuários (users), grupos (groups), funcões (roles) e políticas (policies) e tem as seguintes características:

- Controle centralizado da sua conta AWS
- Acesso compartilhado da sua conta AWs
- Permissões granulares
- Federação de Identidade (Active Directory, Facebook, LinkedIn, etc.)
- Autenticação Multi Fator (MFA)
- Regras de rotação de senhas
- Integração com muitos serviços da AWS
- [PCI DSS Complience](https://www.pluralsight.com/paths/payment-card-industry-data-security-standard-pci-dss)

## Entendendo as entidades

O IAM possui 4 entidades de gerenciamento, são elas:
- Usuários
- Grupos
- Roles/Funcões
- Policies/Políticas

## Usuário
Usuário final. Geralmente pessoas que acessam sua conta e serviços da AWS.

## Grupos
Coletivo de usuários. Todos usuários do grupo irão herdar as permissões daquele grupo.

## Roles/Funcões

## Policies/Políticas
Documentos que especificam permissões em um serviço. São em formato JSON e podem ser adicionados a entidades acima.

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
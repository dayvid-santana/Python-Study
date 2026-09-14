---
tipo: conceito
tags: [arquitetura, persistencia]
---

# Repository Pattern

## Por que existe?

O repositório impede que um caso de uso conheça SQL, ORM ou um fornecedor de dados. Isso protege a regra de negócio de detalhes de persistência e permite teste e substituição mais localizados.

## Como funciona

Ele oferece operações expressas na linguagem do domínio e delega o armazenamento a uma implementação de infraestrutura. O contrato deve refletir necessidades reais do caso de uso, não um CRUD genérico automático.

## Quando não usar

Para scripts pequenos ou CRUD direto sem regra de domínio, a abstração pode só esconder a simplicidade. Registre o motivo ao adotá-la em um [[ADR]].

## Onde aparece no projeto?

Mapeie interfaces, implementações, entidades retornadas e limites de transação. Relacionado: [[Dependency Injection]] · [[Unit of Work]] · [[Dependências entre Camadas]].

> [!note] Material complementar
> Há uma introdução prática em [[Python/12 - Repository Pattern]].

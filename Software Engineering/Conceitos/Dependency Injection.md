---
tipo: conceito
tags: [arquitetura, desacoplamento]
---

# Dependency Injection

## Por que existe?

Uma classe que instancia diretamente seu banco, cliente HTTP ou relógio escolhe sua própria infraestrutura e se torna difícil de testar ou substituir. Injeção de dependência desloca essa escolha para a composição da aplicação.

## Como funciona

O componente recebe o contrato de que precisa — normalmente por construtor, função ou container — e não a implementação concreta. Assim, o caso de uso depende de uma capacidade, não de PostgreSQL ou de uma API específica.

## Quando usar

Use quando houver detalhe externo variável, teste isolado ou fronteira de arquitetura. Não introduza container complexo para objetos simples sem variação relevante.

## Onde aparece no projeto?

Ao analisar um projeto, registre aqui os pontos de composição, contratos e implementações. Relacione-os a [[Repository Pattern]], [[Provider]] e [[Architecture Overview]].

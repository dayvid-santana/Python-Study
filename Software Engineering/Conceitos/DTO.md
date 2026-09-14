---
tipo: conceito
tags: [contratos, dados]
---

# DTO

## Por que existe?

Um *Data Transfer Object* torna explícito o contrato de dados entre fronteiras. Ele evita vazar a estrutura interna do domínio para uma API, fila ou outro módulo.

## Como funciona

O DTO carrega dados de entrada ou saída; regra de negócio continua em entidades ou casos de uso. Ele pode ser convertido de/para schemas e modelos persistidos conforme cada fronteira.

## Onde aparece no projeto?

Registre DTOs por fluxo: origem, validação, transformação e consumidor. Relacione-os a [[Fluxo de Dados]] e às notas de mudança.

> [!note] Material complementar
> Veja [[Python/11 - DTOs e schemas]] para um exemplo em Python.

---
tipo: conceito
tags: [persistencia, transacao]
---

# Unit of Work

## Por que existe?

Quando um caso de uso altera vários agregados ou repositórios, a aplicação precisa decidir o que é uma mudança atômica. Unit of Work delimita essa fronteira e coordena commit ou rollback.

## Como funciona

O caso de uso opera sobre repositórios associados a uma unidade transacional; no final, confirma ou desfaz o conjunto. A abstração não elimina limites de transações distribuídas nem garante idempotência de integrações externas.

## Onde aparece no projeto?

Registre quem abre a unidade, quais repositórios participam e que efeitos externos exigem compensação. Relacionado: [[Repository Pattern]] · [[ADR]].

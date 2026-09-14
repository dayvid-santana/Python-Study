---
tipo: principio
tags: [manutencao, agents-ia, engenharia-software]
---

# Developer Comprehension

Compreensão do desenvolvedor é um requisito de engenharia: código produzido por agents deve continuar compreensível pelos humanos responsáveis por mantê-lo.

## Critério mínimo

Uma implementação significativa precisa permitir identificar:

```text
Intent → Flow → Dependencies → Architecture → Risks → Tests
```

Isso orienta documentação, design e revisão. Se a única forma de modificar algo é pedir nova explicação ao agent, a equipe perdeu autonomia operacional.

## Aplicação prática

Para cada mudança relevante, registre o porquê, o fluxo, contratos, dependentes, riscos e testes no [[Template - Mudança de Código]]. Priorize integrações, regras de negócio, persistência, autenticação e abstrações — não funções triviais.

## Perguntas

1. Um novo mantenedor consegue localizar o ponto certo de mudança?
2. Ele saberia qual teste atualizar antes de alterar a implementação?

Relacionado: [[Code Understanding Layer]] · [[Documentação Viva]] · [[Human in the Loop]]

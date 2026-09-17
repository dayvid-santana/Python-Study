---
tipo: conceito
tags: [agents-ia, agent-harness, role]
---

# Composição de Roles

## Em uma frase

Composição de Roles combina perspectivas compatíveis para uma única execução de [[Agent]].

## Exemplo

```text
Backend Engineer
        +
API Security Reviewer
        ↓
Implementar endpoint com foco extra em autenticação e exposição de dados
```

Uma composição pode usar um Role base, modificadores e uma regra de precedência:

```text
Role base → define responsabilidade primária
Modifier  → adiciona foco delimitado
Policy    → vence qualquer instrução incompatível
```

## Quando usar

Use quando os papéis compartilham objetivo e contexto. Se cada perspectiva exige investigação profunda, resultado independente ou Tools diferentes, prefira um [[SubAgent]] ou [[Workflow]].

## Limitações

Muitos Roles acumulados geram instruções contraditórias e gastam [[Role e Context Window|contexto]]. Defina prioridade explícita; não carregue “todos os especialistas” por padrão.

## Relações

- [[Role]]
- [[Roles Dinâmicos]]
- [[Role vs SubAgent]]
- [[Role e Policies]]

## Lembre-se

**Compor é adicionar foco; delegar é criar trabalho independente.**

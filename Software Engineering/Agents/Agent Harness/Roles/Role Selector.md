---
tipo: conceito
tags: [agents-ia, agent-harness, role, roteamento]
---

# Role Selector

## Em uma frase

Um Role Selector escolhe qual [[Role]] é mais apropriado para a tarefa atual.

```text
Task
  ↓
Role Selector
  ↓
Role apropriado
  ↓
Agent
```

## Formas de seleção

- **Explícita:** usuário escolhe “revisar como Security Reviewer”.
- **Por regras:** arquivos `*.sql` selecionam `Database Reviewer`.
- **Pelo [[Orchestrator]]:** ele escolhe após decompor o objetivo.
- **Por classificação:** um classificador ou LLM mapeia intenção para Role.

## Exemplo

```text
Tarefa: “Avalie este diff de autenticação.”
Regra: contém auth/ ou permissão → Security Reviewer
```

O seletor deve retornar uma decisão explicável e um fallback, como `General Software Engineer`. Para tarefas de alto risco, uma regra determinística ou aprovação humana costuma ser melhor que seleção livre pelo modelo.

## Relações

- [[Role]]
- [[Roles Dinâmicos]]
- [[Workflow]]
- [[Role e Policies]]

## Lembre-se

**Selecionar Role é roteamento de perspectiva, não concessão de permissão.**

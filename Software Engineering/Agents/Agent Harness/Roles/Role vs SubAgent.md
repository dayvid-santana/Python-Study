---
tipo: comparacao
tags: [agents-ia, agent-harness, role, subagents]
---

# Role vs SubAgent

## Em uma frase

Um [[Role]] muda a perspectiva do mesmo [[Agent]]; um [[SubAgent]] delega uma subtarefa para outra execução.

| Trocar Role | Delegar a SubAgent |
| --- | --- |
| Um loop e um resultado principal. | Novo loop, contexto, orçamento e resultado. |
| Útil para mudar prioridade/foco. | Útil para pesquisa, paralelismo ou investigação independente. |
| Geralmente compartilha Tools e sessão. | Pode ter Tools, modelo e permissões próprios. |

## Exemplo

```text
Mesmo Agent + Role Backend
    → implementar endpoint

Orchestrator
├── Backend SubAgent  → implementar
├── Security SubAgent → revisar auth
└── QA SubAgent       → investigar testes
```

## Decisão prática

Troque o Role quando a mudança é de lente e a tarefa segue una. Use SubAgent quando a subtarefa pode ser feita e entregue separadamente, com escopo e custo próprios.

## Relações

- [[Agent]]
- [[Role vs Agent]]
- [[Composição de Roles]]
- [[Orchestrator]]
- [[Workflow]]

## Lembre-se

**Role é reorientação; SubAgent é delegação.**

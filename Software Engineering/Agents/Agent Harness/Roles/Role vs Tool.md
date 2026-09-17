---
tipo: comparacao
tags: [agents-ia, agent-harness, role, tools]
---

# Role vs Tool

## Em uma frase

[[Role]] orienta decisões; [[Tool]] executa uma capacidade controlada.

```text
Role
  ↓ orienta
Skill
  ↓ define procedimento
Tool
  ↓ executa
Sistema externo
```

| Role | Tool |
| --- | --- |
| Perspectiva para raciocínio e priorização. | Interface para ler arquivo, rodar teste ou chamar Git. |
| Não produz efeito externo por si só. | Pode produzir efeito e exige validação. |
| Não concede acesso. | É limitada por [[Policy|Policies]] e sandbox. |

## Exemplo

Um Role `Backend Engineer` pode priorizar testes de integração. A Tool `testing.run` efetivamente executa `pytest`; a permissão para isso não vem do Role.

## Lembre-se

**Role diz por que/quando agir; Tool realiza a ação permitida.**

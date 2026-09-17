---
tipo: relacao
tags: [agents-ia, agent-harness, role, seguranca]
---

# Role e Policies

## Em uma frase

[[Role]] define comportamento esperado; [[Policy]] define o que é permitido.

```text
Role
  ↓ define foco e critérios
Policy
  ↓ permite, bloqueia ou exige aprovação
Tool
  ↓ executa a ação autorizada
```

| Role | Policy |
| --- | --- |
| “Atue como Git Maintainer.” | “`git push` exige aprovação humana.” |
| Orienta escolhas e linguagem. | É regra verificável pelo Harness. |
| Pode sugerir uma ação. | Decide se a ação pode acontecer. |

## Exemplo

Mesmo que o Role `Database Engineer` peça uma migração, a Policy pode bloquear escrita fora do ambiente de teste ou exigir aprovação para comandos destrutivos.

## Por que importa

Prompt/Role podem ser interpretados incorretamente, substituídos por contexto hostil ou simplesmente ignorados pelo modelo. A Policy precisa estar no caminho de execução da [[Tool]], idealmente com sandbox e validação de argumentos.

## Relações

- [[Role]]
- [[Role vs Tool]]
- [[Role Selector]]
- [[Policy]]
- [[Human in the Loop]]

## Lembre-se

**Role não é autorização. Permissão vem de Policy aplicada tecnicamente.**

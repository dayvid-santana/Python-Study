---
tipo: comparacao
tags: [agents-ia, agent-harness, role, prompts]
---

# Role vs Prompt

## Em uma frase

Um [[Role]] é uma abstração de comportamento; um [[Prompt Engineering|Prompt]] é o veículo textual que pode expressá-la.

| Role | Prompt |
| --- | --- |
| É um conceito de arquitetura e domínio. | É uma mensagem ou bloco de mensagens enviado ao modelo. |
| Define identidade de trabalho, critérios e escopo. | Pode conter Role, tarefa, formato de saída e exemplos. |
| Pode existir como configuração estruturada. | É uma forma comum de materializar a configuração. |

## Exemplo

```markdown
<!-- trecho de prompt que materializa um Role -->
Você atua como Security Reviewer.
Priorize riscos exploráveis, evidências no diff e mitigações objetivas.
```

O Harness pode compor esse texto a partir de um `ROLE.md`, da tarefa e das [[Policy|Policies]]. Portanto, não é preciso confundir o arquivo de prompt inteiro com o Role.

## Relações

- [[Role]]
- [[ROLE.md como configuração]]
- [[Role e Context Engineering]]
- [[Prompt Engineering]]

## Lembre-se

**Prompt carrega instruções; Role organiza a intenção dessas instruções.**

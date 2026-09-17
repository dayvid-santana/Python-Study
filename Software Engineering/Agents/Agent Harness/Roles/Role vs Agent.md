---
tipo: comparacao
tags: [agents-ia, agent-harness, role]
---

# Role vs Agent

## Em uma frase

O [[Agent]] é a unidade que trabalha; o [[Role]] é a perspectiva que orienta seu trabalho.

| Agent | Role |
| --- | --- |
| Recebe objetivo, contexto e executa um loop. | Define responsabilidades e prioridades para a tarefa. |
| Pode chamar [[Tool|Tools]] sob controle do Harness. | Pode recomendar Tools, mas não as executa. |
| Tem ciclo de vida e resultado próprios. | Pode ser carregado, trocado ou combinado. |

## Exemplo

```text
Mesmo Agent
├── Role: Backend Engineer → implementa endpoint e testes
└── Role: Security Reviewer → analisa autenticação e dados expostos
```

## Quando separar

Troque apenas o Role quando o objetivo e as Tools continuam semelhantes, mas mudam os critérios de análise. Crie Agents distintos quando cada papel exige ciclo de vida, modelo, permissões, contexto ou avaliação próprios.

Veja também [[Role vs SubAgent]].

## Lembre-se

**Role é configuração de comportamento; Agent é unidade de execução.**

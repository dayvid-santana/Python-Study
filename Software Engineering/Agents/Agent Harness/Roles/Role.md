---
tipo: conceito
tags: [agents-ia, agent-harness, role]
---

# Role

## Em uma frase

Um Role define **como um [[Agent]] deve abordar uma categoria de tarefa**.

## Conceito

Ele fornece responsabilidades, prioridades, critérios, terminologia e limites de atuação. Pode apontar [[Skill|Skills]] e [[Tool|Tools]] relevantes, mas não cria novas capacidades no modelo nem concede permissões.

Um mesmo Agent pode atuar como `Backend Engineer` em uma implementação ou `Security Reviewer` em uma revisão.

```text
Agent + Role + Skills + Context + Policies + Task
                    ↓
             execução especializada
```

## Exemplo

```text
Role: Security Reviewer
Prioridade: vulnerabilidades e exposição de dados
Critério: apontar risco, evidência e mitigação
```

Esse Role não transforma literalmente o modelo em especialista; ele orienta seu foco e linguagem durante a execução.

## Vantagens e limites

- **Vantagem:** permite especializar o mesmo Agent sem duplicar todo o runtime.
- **Vantagem:** deixa critérios de revisão e linguagem de saída explícitos e revisáveis.
- **Limite:** não substitui conhecimento real recuperado, [[Skill|Skills]], testes ou validação humana.
- **Limite:** Roles longos ou contraditórios consomem contexto e reduzem clareza; veja [[Role e Context Window]].

## Relações

- [[Role vs Agent]]
- [[Role vs Skill]]
- [[Role Selector]]
- [[Role e Context Engineering]]
- [[Role e Policies]]

## Lembre-se

**Agent executa; Role orienta como ele atua.**

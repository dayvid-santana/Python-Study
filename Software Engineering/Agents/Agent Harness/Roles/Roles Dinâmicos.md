---
tipo: conceito
tags: [agents-ia, agent-harness, role]
---

# Roles Dinâmicos

## Em uma frase

Roles dinâmicos são [[Role|Roles]] selecionados, parametrizados ou compostos em tempo de execução.

## Como funcionam

Em vez de anexar sempre o mesmo texto, o Harness usa dados da tarefa, projeto e usuário para escolher um Role base e preencher limites relevantes.

```text
Tarefa + tipo de repositório + risco
              ↓
  Role base: Backend Engineer
              +
  parâmetros: Python, API pública, sem escrita em produção
```

## Exemplo

`Code Reviewer` pode receber foco dinâmico em `performance`, `segurança` ou `acessibilidade` conforme os arquivos alterados.

## Cuidados

- Validar valores em vez de interpolar texto não confiável.
- Limitar a composição a Roles/atributos permitidos.
- Registrar qual Role efetivamente foi usado para depuração e [[evals]].
- Não gerar permissões a partir do Role; aplique [[Role e Policies]].

## Relações

- [[Role Selector]]
- [[Composição de Roles]]
- [[Role e Context Window]]

## Lembre-se

**Dinâmico é adaptável, não ilimitado: o Harness continua sendo a fronteira de controle.**

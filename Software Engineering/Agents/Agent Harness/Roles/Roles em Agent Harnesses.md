---
tipo: indice
tags: [agents-ia, agent-harness, roles]
---

# Roles em Agent Harnesses

> [!summary]
> Um [[Role]] é uma perspectiva de trabalho que orienta um [[Agent]] para uma categoria de tarefa. É uma abstração arquitetural útil, não uma exigência de todo [[Agent Harness]].

## Conceitos fundamentais

- [[Role]] — responsabilidades, prioridades e critérios.
- [[Agent]] — unidade que raciocina e executa uma tarefa.
- [[Skill]] — conhecimento procedimental reutilizável.
- [[Tool]] — capacidade controlada que causa ou consulta efeitos externos.
- [[Prompt Engineering]] — técnica para estruturar as instruções do modelo.

## Funcionamento e composição

- [[Role Selector]] — escolhe o Role adequado à tarefa.
- [[Roles Dinâmicos]] — monta ou altera Roles em tempo de execução.
- [[Composição de Roles]] — combina perspectivas compatíveis.
- [[ROLE.md como configuração]] — representa um Role em Markdown.

## Comparações importantes

- [[Role vs Agent]]
- [[Role vs Skill]]
- [[Role vs Prompt]]
- [[Role vs Tool]]
- [[Role vs SubAgent]]

## Contexto, controle e custo

- [[Role e Context Engineering]]
- [[Role e Context Window]]
- [[Role e Policies]]

## Mapa rápido

```text
Task
  ↓
Role Selector
  ↓
Role + Agent + Skills + Context + Policies
  ↓
Execução especializada
  ↓
Tools autorizadas
```

## Lembre-se

**Role orienta a abordagem; o Harness ainda controla contexto, permissões e execução.**

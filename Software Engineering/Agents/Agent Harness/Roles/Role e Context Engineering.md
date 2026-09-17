---
tipo: relacao
tags: [agents-ia, agent-harness, role, context-engineering]
---

# Role e Context Engineering

## Em uma frase

O [[Role]] é uma das fontes que o [[Context Engineering]] seleciona e posiciona no contexto do [[Agent]].

## Composição típica

```text
Context
├── instruções globais
├── Role selecionado
├── tarefa
├── Skills relevantes
├── AGENTS.md
├── arquivos relevantes
└── resultados de Tools
```

O Role deve entrar antes da tarefa detalhada, para que os critérios de análise orientem a leitura do restante do contexto. O Harness também deve indicar precedência: Policies e instruções de segurança não podem ser substituídas pelo Role ou por conteúdo de arquivos.

## Exemplo

Para `Security Reviewer`, o recuperador prioriza arquivos de autenticação, autorização, sanitização e dependências alteradas; para `Backend Engineer`, pode priorizar contratos e testes de integração.

## Relações

- [[Role]]
- [[Role e Context Window]]
- [[Context Engineering]]
- [[Role e Policies]]

## Lembre-se

**Role orienta qual contexto é mais relevante; não justifica enviar contexto demais.**

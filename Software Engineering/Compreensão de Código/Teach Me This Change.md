---
tipo: fluxo-de-trabalho
tags: [agents-ia, aprendizado, code-understanding]
---

# Teach Me This Change

## Ideia

Cada mudança pode virar uma aula contextualizada. Um modo futuro de harness, como `dev-agent explain` ou `dev-agent explain --last-commit`, deve produzir explicação suficiente para manutenção, não apenas um resumo do commit.

```text
O problema → A solução → O fluxo → A arquitetura
→ Os conceitos → Os riscos → Os testes → Perguntas de compreensão
```

## Contrato de saída

1. Problema e contexto da tarefa.
2. Solução e por que foi escolhida.
3. Fluxos de execução e dados afetados.
4. Arquivos, dependências e decisões arquiteturais.
5. Conceitos novos ou críticos.
6. Riscos, testes e validação manual.
7. Perguntas que comprovem compreensão ativa.

> [!example] Pergunta útil
> Se a persistência fosse substituída, quais arquivos deveriam mudar e quais deveriam permanecer intactos?

O resultado pode ser criado a partir do [[Template - Mudança de Código]] e revisado pelo [[Explainer Agent]].

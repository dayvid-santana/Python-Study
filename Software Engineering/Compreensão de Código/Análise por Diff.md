---
tipo: pratica
tags: [git, revisao, code-understanding]
---

# Análise por Diff

## Por que o diff vem primeiro?

O diff delimita a hipótese de mudança: ele mostra a intenção materializada e evita reler o projeto inteiro. Ainda assim, linhas alteradas raramente bastam; a revisão deve expandir para chamadores, contratos e testes impactados.

```text
Objetivo da tarefa → Diff → Arquivos alterados → Fluxo impactado
→ Arquitetura impactada → Testes → Riscos
```

## Comandos úteis

```bash
git diff           # alterações ainda não preparadas
git diff --staged  # alterações preparadas
git show           # commit e seu diff
git show --stat    # escopo resumido do commit
git log --oneline  # histórico compacto
```

## Como interpretar

Leia cada arquivo perguntando: qual contrato mudou? A alteração atravessa uma fronteira de camada? Há código removido que algum consumidor ainda espera? Um teste cobre o comportamento, não apenas a linha nova? Depois, siga o [[Fluxo de Execução]] e o [[Fluxo de Dados]] afetados.

> [!tip] Heurística
> Mudança pequena em autenticação, persistência ou contratos públicos pode ser mais relevante que muitas linhas de interface.

## Relacionado

[[Explainer Agent]] · [[Complexity Radar]] · [[Revisão de Código Gerado por IA]]

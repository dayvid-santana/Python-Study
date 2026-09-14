---
tipo: pratica
tags: [risco, revisao, agents-ia]
---

# Complexity Radar

## Por que classificar complexidade?

A precisão matemática não é o objetivo. O radar direciona atenção humana para pontos cujo erro custa caro ou é difícil de detectar e reverter.

```text
Complexidade geral: 7/10

CRÍTICO: autenticação, sessão, concorrência
IMPORTANTE: Dependency Injection, Repository Pattern, erros
SECUNDÁRIO: dataclasses, typing, imports
```

## Como avaliar

Considere impacto no negócio, segurança, número de dependências e arquivos, algoritmo, concorrência, infraestrutura, persistência, autenticação, integração externa, dificuldade de teste e facilidade de reversão.

## Uso na revisão

- **Crítico:** exija cenário de falha, revisão humana profunda e testes representativos.
- **Importante:** confirme contratos, limites de camada e regressões prováveis.
- **Secundário:** mantenha legibilidade e consistência, sem gastar esforço desproporcional.

[[Análise por Diff]] revela o escopo; este radar decide onde aprofundar.

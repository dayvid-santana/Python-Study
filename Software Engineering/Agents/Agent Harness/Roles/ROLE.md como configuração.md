---
tipo: padrao-de-representacao
tags: [agents-ia, agent-harness, role, markdown]
---

# ROLE.md como configuração

## Em uma frase

Um `ROLE.md` é uma forma legível e versionável de representar a configuração de um [[Role]].

## Organização possível

```text
roles/
├── software/
│   ├── backend-engineer/
│   │   └── ROLE.md
│   ├── architect/
│   │   └── ROLE.md
│   └── security-reviewer/
│       └── ROLE.md
└── design/
    └── graphic-designer/
        └── ROLE.md
```

Não é um padrão universal. Um Harness pode usar YAML, JSON ou código tipado; Markdown é especialmente útil quando pessoas revisam as instruções e precisam anexar exemplos/referências.

## Estrutura mínima

```markdown
# Backend Engineer

## Responsabilidade
Implementar e manter serviços de backend.

## Prioridades
Contratos, testes, legibilidade e compatibilidade.

## Princípios
Prefira mudanças pequenas e verificáveis.

## Restrições
Não altere migrações sem aprovação.

## Skills relacionadas
- [[Python]]
- [[pytest]]

## Tools relacionadas
- `filesystem.read`
- `testing.run`
```

## Implementação segura

O carregador deve tratar `ROLE.md` como dados confiáveis e versionados, aplicar limites de tamanho, validar metadados e compor o conteúdo com [[Role e Policies|Policies]] fora do arquivo. Não permita que o Role declare sua própria permissão de escrita/rede.

## Relações

- [[Role]]
- [[Role vs Prompt]]
- [[Skills]]
- [[Tools]]
- [[Role e Context Engineering]]

## Lembre-se

**`ROLE.md` documenta a orientação; o Harness valida e impõe os limites.**

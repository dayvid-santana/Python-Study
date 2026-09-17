---
tipo: relacao
tags: [agents-ia, agent-harness, role, tokens]
---

# Role e Context Window

## Em uma frase

Cada [[Role]] consome parte da janela de contexto e deve ter tamanho proporcional ao ganho de especialização.

## Impacto

```text
Janela disponível
├── instruções globais
├── Role
├── tarefa e histórico
├── Skills
├── arquivos/resultados
└── reserva para resposta
```

Carregar Roles extensos, vários papéis incompatíveis ou Skills irrelevantes reduz espaço para o problema real, aumenta custo e pode confundir prioridades.

## Práticas úteis

- Carregue só o Role escolhido pelo [[Role Selector]].
- Use um Role base curto e referências recuperáveis para detalhes.
- Componha Roles apenas quando há necessidade clara.
- Reserve tokens para Tool results e resposta final.
- Resuma histórico antigo; não resuma regras de segurança críticas sem validação.

## Exemplo

Em uma correção de bug de 20 linhas, um Role de 40 linhas e checklist de 200 linhas pode custar mais contexto do que os arquivos necessários para resolver o defeito. Carregue o núcleo e recupere o checklist sob demanda.

## Relações

- [[Role]]
- [[Role e Context Engineering]]
- [[Composição de Roles]]
- [[Token Management]]

## Lembre-se

**Especialização útil é seletiva: contexto é um orçamento, não um depósito.**

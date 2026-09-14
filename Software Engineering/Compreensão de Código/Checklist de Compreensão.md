---
tipo: checklist
tags: [code-understanding, revisao]
---

# Checklist de Compreensão

Antes de aprovar ou manter uma implementação relevante, responda:

1. Qual problema está sendo resolvido?
2. Onde o fluxo começa e termina?
3. Quais dados entram e saem?
4. Onde ocorrem parsing, validação e transformação?
5. Onde está a regra de negócio e a persistência?
6. Quais serviços ou componentes externos participam?
7. Por que as abstrações foram usadas?
8. Quais módulos dependem disso e o que pode quebrar?
9. Como testar e detectar regressão?
10. Que conceitos preciso dominar e onde uma mudança futura deveria ocorrer?

Use junto com [[Análise por Diff]] e [[Template - Mudança de Código]].

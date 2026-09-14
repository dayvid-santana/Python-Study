---
tipo: pratica
tags: [code-understanding, dados]
---

# Fluxo de Dados

## Por que acompanhar dados?

Em sistemas desconhecidos, o dado expõe contratos e fronteiras melhor que nomes de classes. Pergunte qual formato entra, qual invariável é garantida em cada etapa e qual dado sai.

```text
Origem → Entrada → Parsing → Validação → Transformação
       → Regra de negócio → Persistência → Saída
```

## Diferença para o fluxo de execução

[[Fluxo de Execução]] acompanha **quem chama quem**. Fluxo de dados acompanha **o que é transportado, transformado e persistido**. Os dois se cruzam, mas não são a mesma análise.

## Pontos de inspeção

- formato e confiança da entrada;
- parsing e validação na fronteira;
- conversão entre schema, [[DTO]] e entidade;
- invariantes da regra de negócio;
- representação persistida e resposta pública.

## Perguntas de compreensão

1. Onde um dado inválido é rejeitado?
2. Qual transformação é irreversível ou perde informação?
3. Que contrato externo seria quebrado se a saída mudasse?

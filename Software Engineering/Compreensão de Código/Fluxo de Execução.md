---
tipo: pratica
tags: [code-understanding, fluxo]
---

# Fluxo de Execução

## Por que seguir chamadas?

O fluxo revela onde cada responsabilidade vive. Em vez de ler arquivos isolados, acompanhe uma requisição desde sua entrada até o efeito observável.

```text
POST /usuarios
        ↓
Controller → Service → Domain → Repository → Database
```

O desenho é um exemplo, não uma regra universal: um projeto pode usar filas, funções, eventos ou uma camada de aplicação com outros nomes.

## Como investigar

1. Encontre a entrada: rota, comando, consumidor de evento ou job.
2. Siga cada chamada até a saída, anotando as fronteiras entre camadas.
3. Marque onde há regra de negócio, I/O, transformação, tratamento de erro e término.
4. Compare o caminho com os testes de integração e unitários.

## Perguntas

- Onde a execução começa e qual função chama a próxima?
- Onde está a regra de negócio?
- Onde ocorre I/O e onde os erros são tratados?
- Onde os dados são transformados e onde termina o fluxo?

Veja também [[Fluxo de Dados]] e [[Dependências entre Camadas]].

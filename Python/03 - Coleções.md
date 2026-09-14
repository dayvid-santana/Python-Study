# Coleções

## Lista — ordenada e mutável

```python
cores = ["azul", "verde"]
cores.append("roxo")
cores[0] = "vermelho"
cores.pop()           # remove e retorna o último
```

Copie com `cores.copy()` ou `cores[:]`; `outra = cores` aponta para a mesma lista.

## Tupla — ordenada e imutável

```python
coordenada = (10, 20)
x, y = coordenada
```

Boa para dados fixos e retornos múltiplos.

## Dicionário — chave → valor

```python
aluno = {"nome": "Ana", "nota": 9}
aluno["nota"] = 10
aluno.get("turma", "sem turma")

for chave, valor in aluno.items():
    print(chave, valor)
```

Chaves devem ser imutáveis, como `str`, `int` ou `tuple`.

## Conjunto (`set`) — valores únicos

```python
tags = {"python", "web", "python"}  # {"python", "web"}
tags.add("api")
comuns = tags & {"api", "dados"}
```

Use para remover duplicatas ou testar pertencimento rapidamente: `item in conjunto`.

## Escolha rápida

| Preciso de… | Use |
| --- | --- |
| sequência editável | `list` |
| registro fixo | `tuple` |
| dados nomeados | `dict` |
| unicidade | `set` |

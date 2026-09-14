# Receitas frequentes

## Ler um dicionário com valor padrão

```python
quantidade = carrinho.get("maçã", 0)
```

## Contar ocorrências

```python
from collections import Counter

contagem = Counter(["a", "b", "a"])
contagem["a"]  # 2
```

## Ordenar sem alterar a coleção

```python
por_nome = sorted(alunos, key=lambda aluno: aluno["nome"])
```

`list.sort()` altera a lista e retorna `None`; `sorted()` retorna uma nova lista.

## Filtrar valores nulos

```python
preenchidos = [valor for valor in valores if valor is not None]
```

Use `is not None` quando `0`, `False` ou `""` também forem valores válidos.

## Percorrer duas listas

```python
for nome, nota in zip(nomes, notas):
    print(nome, nota)
```

## Caminhos portáveis

```python
from pathlib import Path

arquivo = Path("dados") / "entrada.csv"
```

## Data e hora atual

```python
from datetime import datetime

agora = datetime.now()
```

Para sistemas com fusos, prefira `datetime.now().astimezone()` ou `zoneinfo.ZoneInfo`.

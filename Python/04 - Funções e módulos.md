# Funções e módulos

## Função

```python
def saudacao(nome: str, entusiasmo: bool = False) -> str:
    fim = "!" if entusiasmo else "."
    return f"Olá, {nome}{fim}"
```

Parâmetros recebem dados; `return` devolve um resultado. Sem `return`, o resultado é `None`. Anotações de tipo ajudam pessoas e ferramentas, mas não validam sozinhas em execução.

## Argumentos úteis

```python
def total(*numeros, desconto=0, **opcoes):
    return sum(numeros) - desconto

total(10, 20, desconto=5)
```

- `*args`: argumentos posicionais extras, em uma tupla.
- `**kwargs`: argumentos nomeados extras, em um dicionário.

Não use lista/dicionário como valor padrão mutável. Prefira `None`:

```python
def adicionar(item, itens=None):
    if itens is None:
        itens = []
    itens.append(item)
    return itens
```

## Escopo

Uma variável criada dentro da função é local. Leia variáveis externas quando necessário; evite modificá-las.

## Módulos

```python
from pathlib import Path
import math

math.sqrt(9)
Path("dados.txt").exists()
```

Em seu projeto, um arquivo `calculos.py` vira o módulo `calculos`: `from calculos import somar`.

> Atenção: deixe código executável de teste sob `if __name__ == "__main__":`.

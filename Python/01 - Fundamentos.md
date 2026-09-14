# Fundamentos

## Variáveis e tipos

```python
nome = "Ana"          # str
idade = 20            # int
altura = 1.68         # float
estuda = True         # bool
nada = None           # ausência de valor
```

Python descobre o tipo em tempo de execução. Consulte com `type(valor)`.

## Texto (`str`)

```python
nome = "ana silva"
nome.title()          # "Ana Silva"
nome.upper()          # "ANA SILVA"
nome.strip()          # remove espaços nas pontas
nome.replace("ana", "bia")
f"Olá, {nome}!"      # interpolação
```

Índices começam em zero: `"Python"[0]` é `"P"`. Fatias não incluem o fim: `texto[0:3]`.

## Números e operadores

```python
7 / 2     # 3.5   divisão real
7 // 2    # 3     divisão inteira
7 % 2     # 1     resto
2 ** 3    # 8     potência
```

Comparações retornam `True` ou `False`: `==`, `!=`, `<`, `<=`, `>`, `>=`.

## Converter e receber dados

```python
idade = int("20")
preco = float("9.90")
texto = str(42)
nome = input("Seu nome: ")  # sempre retorna str
```

> Atenção: `==` compara valores; `is` compara identidade. Para ausência de valor, use `valor is None`.

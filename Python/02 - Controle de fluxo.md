# Controle de fluxo

## Condições

```python
if nota >= 7:
    situacao = "aprovado"
elif nota >= 5:
    situacao = "recuperação"
else:
    situacao = "reprovado"
```

Use indentação de quatro espaços. Valores vazios, `0`, `False` e `None` são falsos em uma condição.

```python
mensagem = "maior" if idade >= 18 else "menor"
```

## `for` e `range`

```python
for fruta in ["maçã", "uva"]:
    print(fruta)

for numero in range(1, 6):  # 1 até 5
    print(numero)
```

`enumerate(itens, start=1)` entrega índice e valor. `zip(a, b)` percorre iteráveis em pares.

## `while`, `break` e `continue`

```python
tentativas = 0
while tentativas < 3:
    tentativas += 1
    if tentativas == 2:
        continue  # pula para a próxima volta
    print(tentativas)
```

`break` encerra o laço imediatamente. Use `else` em um laço quando quiser executar algo apenas se não houve `break`.

## Compreensão de lista

```python
quadrados = [n ** 2 for n in range(10)]
pares = [n for n in range(10) if n % 2 == 0]
```

> Atenção: se a compreensão ficar difícil de ler, prefira um `for` comum.

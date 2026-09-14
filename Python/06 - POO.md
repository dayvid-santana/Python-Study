# POO — classes e objetos

## Classe básica

```python
class Conta:
    def __init__(self, titular, saldo=0):
        self.titular = titular
        self.saldo = saldo

    def depositar(self, valor):
        if valor <= 0:
            raise ValueError("valor inválido")
        self.saldo += valor
```

`Conta` é o molde; `Conta("Ana")` cria um objeto. `self` é o próprio objeto e aparece em métodos de instância.

## `dataclass` para dados

```python
from dataclasses import dataclass

@dataclass
class Produto:
    nome: str
    preco: float
```

Ela cria automaticamente `__init__`, representação e comparação para classes cujo foco é armazenar dados.

## Herança, com moderação

```python
class Animal:
    def falar(self):
        return "..."

class Cachorro(Animal):
    def falar(self):
        return "au"
```

Prefira composição quando um objeto **tem um** componente; herança quando ele **é um** tipo especializado.

> Atenção: encapsulamento em Python é convenção. Um atributo `_interno` indica “não use fora da classe”.

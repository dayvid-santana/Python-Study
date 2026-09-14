# Modelagem de domínio

## Ideia central

O **domínio** é a parte do problema que o software resolve: pedidos, alunos, faturas, estoque, agendamentos etc.

Modelar o domínio é dar nomes e regras do negócio a objetos do código. Em vez de “um dicionário com dados”, use um objeto que saiba proteger suas próprias regras.

## Exemplo: carrinho de compras

```python
from dataclasses import dataclass, field
from decimal import Decimal

@dataclass(frozen=True)
class Item:
    nome: str
    preco: Decimal

    def __post_init__(self):
        if self.preco < 0:
            raise ValueError("preço não pode ser negativo")

@dataclass
class Carrinho:
    itens: list[Item] = field(default_factory=list)

    def adicionar(self, item: Item) -> None:
        self.itens.append(item)

    def total(self) -> Decimal:
        return sum((item.preco for item in self.itens), start=Decimal("0"))
```

```python
livro = Item("Python", Decimal("49.90"))
carrinho = Carrinho()
carrinho.adicionar(livro)
print(carrinho.total())  # 49.90
```

Aqui, `Item` e `Carrinho` são conceitos do domínio. A regra “preço não pode ser negativo” mora onde faz sentido: no próprio `Item`.

## Perguntas para descobrir o modelo

- Quais são os **substantivos** do problema? → candidatos a classes (`Aluno`, `Pedido`).
- Quais ações eles realizam? → métodos (`pedido.cancelar()`).
- Quais regras nunca podem ser violadas? → validações no modelo.
- Quais dados só identificam ou descrevem algo? → atributos / `dataclass`.

## Onde colocar no projeto

```text
app/
├── __init__.py
├── dominio/
│   ├── __init__.py
│   ├── item.py
│   └── carrinho.py
├── servicos/       # coordena casos de uso
└── infraestrutura/ # banco, API, arquivos
```

```python
# app/dominio/__init__.py
from .item import Item
from .carrinho import Carrinho

# uso externo: import absoluto
from app.dominio import Carrinho, Item
```

O `__init__.py` oferece uma entrada simples para o domínio; os módulos internos continuam organizados por conceito.

> Atenção: não transforme cada tabela do banco em uma classe por reflexo. Comece pelas regras e pela linguagem do problema, não pela tecnologia usada para salvar dados.

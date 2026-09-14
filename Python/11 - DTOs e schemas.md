# DTOs e schemas

## Três papéis diferentes

| Conceito | Responsabilidade | Exemplo |
| --- | --- | --- |
| Entidade de domínio | regras do negócio | `Carrinho.total()` |
| DTO (*Data Transfer Object*) | levar dados entre partes da aplicação | `CriarItemDTO` |
| Schema | contrato e validação de dados externos | corpo de uma requisição HTTP |

Na prática, frameworks como FastAPI + Pydantic podem usar a mesma classe como DTO e schema. Conceitualmente, porém, a intenção é diferente.

## Schema: a fronteira da aplicação

Um schema verifica dados que chegam de fora antes de criar objetos do domínio.

```python
from decimal import Decimal
from pydantic import BaseModel, Field

class ItemEntrada(BaseModel):
    nome: str = Field(min_length=1)
    preco: Decimal = Field(ge=0)
```

```python
entrada = ItemEntrada(nome="Python", preco="49.90")
item = Item(nome=entrada.nome, preco=entrada.preco)
```

O schema aceita/recusa o formato da entrada. A entidade `Item` continua responsável por sua regra, mesmo que alguém a crie sem passar pela API.

## DTO: dados para um caso de uso

```python
from dataclasses import dataclass
from decimal import Decimal

@dataclass(frozen=True)
class CriarItemDTO:
    nome: str
    preco: Decimal
```

```python
def criar_item(dados: CriarItemDTO) -> Item:
    return Item(nome=dados.nome, preco=dados.preco)
```

DTOs devem ser simples: carregam dados e normalmente não concentram regra de negócio. `dataclass` é suficiente quando os dados já foram validados.

## Fluxo típico

```text
HTTP / JSON → Schema de entrada → DTO → caso de uso → entidade de domínio
entidade de domínio → DTO de saída → Schema de resposta → JSON
```

Exemplo de resposta: não devolva a entidade diretamente se a API tiver um contrato público.

```python
class ItemResposta(BaseModel):
    nome: str
    preco: Decimal

def para_resposta(item: Item) -> ItemResposta:
    return ItemResposta(nome=item.nome, preco=item.preco)
```

## Organização com pacotes

```text
app/
├── dominio/       # Item, Carrinho e regras
├── aplicacao/     # DTOs e casos de uso
└── apresentacao/  # schemas HTTP, rotas
```

Mantenha os `__init__.py` leves. Eles podem expor classes úteis, mas não devem fazer o domínio depender do framework web:

```python
# app/aplicacao/__init__.py
from .dtos import CriarItemDTO
```

> Regra prática: dados externos são validados por schemas; dados atravessam camadas por DTOs; regras vivem nas entidades do domínio.

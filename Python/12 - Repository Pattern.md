# Repository Pattern

## Ideia central

Um **repositório** é a porta de acesso aos objetos do domínio. Ele esconde *como* os dados são salvos ou buscados.

```text
caso de uso → repositório (interface) → memória / banco de dados / API
```

Assim, `CriarPedido` depende de algo que “salva pedidos”, e não de SQL, ORM ou `sqlite3` diretamente.

## Interface do repositório

```python
from typing import Protocol

class PedidoRepositorio(Protocol):
    def salvar(self, pedido: Pedido) -> None: ...
    def buscar_por_id(self, pedido_id: str) -> Pedido | None: ...
```

`Protocol` descreve o contrato. Uma classe não precisa herdar dele: basta implementar os mesmos métodos.

## Implementação simples em memória

```python
class PedidoRepositorioEmMemoria:
    def __init__(self) -> None:
        self._pedidos: dict[str, Pedido] = {}

    def salvar(self, pedido: Pedido) -> None:
        self._pedidos[pedido.id] = pedido

    def buscar_por_id(self, pedido_id: str) -> Pedido | None:
        return self._pedidos.get(pedido_id)
```

## Caso de uso sem saber do banco

```python
class ConsultarPedido:
    def __init__(self, repositorio: PedidoRepositorio) -> None:
        self.repositorio = repositorio

    def executar(self, pedido_id: str) -> Pedido:
        pedido = self.repositorio.buscar_por_id(pedido_id)
        if pedido is None:
            raise LookupError("pedido não encontrado")
        return pedido
```

No teste, passe `PedidoRepositorioEmMemoria()`. Em produção, passe uma implementação com PostgreSQL. O caso de uso não muda.

## Organização sugerida

```text
app/
├── dominio/
│   └── pedido.py             # Pedido e regras
├── aplicacao/
│   └── consultar_pedido.py   # caso de uso + contrato
└── infraestrutura/
    └── pedido_postgres.py    # implementação para o banco
```

Os imports seguem a direção das regras: infraestrutura pode importar domínio; domínio não deve importar banco, ORM ou FastAPI.

## Boas práticas

- Repositório devolve entidades/agregados do domínio, não linhas cruas do banco.
- Dê nomes do domínio: `buscar_por_email`, `listar_abertos`; não crie um CRUD genérico sem necessidade.
- Deixe transações que envolvem vários repositórios para uma futura camada de *Unit of Work*.
- Só aplique o padrão quando houver lógica de domínio, testes ou mais de uma forma de persistência. Para um script CRUD pequeno, uma consulta direta pode ser mais simples.

> `__init__.py` pode expor contratos ou implementações convenientes, mas não deve conectar ao banco durante a importação.

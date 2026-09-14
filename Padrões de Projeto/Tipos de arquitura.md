Em uma empresa de varejo, eu não tentaria aplicar o máximo de padrões possível. Eu escolheria padrões que resolvessem problemas recorrentes do negócio: **estoque, pedidos, pagamentos, promoções, clientes, integrações, filas, concorrência e disponibilidade**.

Uma arquitetura bastante realista seria algo assim:

```text
                        ┌─────────────────────┐
                        │  Site / App / PDV   │
                        └─────────┬───────────┘
                                  │
                            API / BFF
                                  │
            ┌─────────────────────┼─────────────────────┐
            ▼                     ▼                     ▼
        Pedidos                Estoque              Clientes
            │                     │                     │
            ▼                     ▼                     ▼
       Pagamentos             Catálogo             Fidelidade
            │
            ▼
       Integrações
   ERP / Gateway / Logística
```

Eu dividiria os padrões em algumas categorias.

## 1. Clean Architecture ou Hexagonal Architecture

Provavelmente seria minha base para serviços importantes.

Imagine o módulo de pedidos:

```text
pedidos/
├── domain/
│   ├── entities/
│   ├── value_objects/
│   └── services/
│
├── application/
│   └── use_cases/
│
├── infrastructure/
│   ├── database/
│   ├── payment/
│   └── messaging/
│
└── presentation/
    └── api/
```

O domínio não deveria depender diretamente de:

```text
PostgreSQL
Redis
Stripe
SAP
Oracle
RabbitMQ
FastAPI
```

Esses sistemas seriam conectados por adapters.

Isso permite, por exemplo:

```python
class RepositorioPedido(Protocol):
    def salvar(self, pedido: Pedido) -> None:
        ...
```

E depois:

```python
class RepositorioPedidoPostgres:
    ...
```

Isso combina:

- Hexagonal Architecture
    
- Ports and Adapters
    
- Dependency Inversion
    
- Dependency Injection
    

---

# 2. Repository
Eu usaria bastante.

Por exemplo:

```python
pedido = repositorio_pedidos.buscar_por_id(id_pedido)
```

em vez de espalhar:

```python
session.query(Pedido).filter(...)
```

pela aplicação.

Exemplos:

```text
RepositorioPedido
RepositorioCliente
RepositorioProduto
RepositorioEstoque
RepositorioPromocao
```

A aplicação conhece o conceito de repositório, não os detalhes do banco.

---

# 3. Unit of Work

Muito útil em operações comerciais.

Imagine:

```text
Criar pedido
    ↓
Reservar estoque
    ↓
Registrar pagamento
    ↓
Salvar pedido
```

Você não quer:

```text
pedido salvo       ✓
estoque reservado  ✓
pagamento registrado ✗
```

e ficar com dados inconsistentes.

Uma Unit of Work pode coordenar a transação:

```python
with unit_of_work:
    pedido = criar_pedido(...)
    estoque.reservar(...)
    repositorio.salvar(pedido)

    unit_of_work.commit()
```

---

# 4. Strategy

Esse seria um dos padrões que eu mais usaria no varejo.

Porque varejo possui muitas regras variáveis.

Por exemplo, cálculo de desconto:

```text
Desconto
   │
   ├── Cliente comum
   ├── Cliente VIP
   ├── Black Friday
   ├── Cupom
   ├── Funcionário
   └── Programa fidelidade
```

Em vez de:

```python
if cliente.vip:
    ...
elif black_friday:
    ...
elif funcionario:
    ...
```

poderíamos ter:

```python
class EstrategiaDesconto(Protocol):
    def calcular(self, pedido) -> Decimal:
        ...
```

e:

```python
DescontoVip()
DescontoBlackFriday()
DescontoFuncionario()
DescontoCupom()
```

Strategy funciona muito bem para:

- descontos;
    
- frete;
    
- impostos;
    
- cashback;
    
- precificação;
    
- parcelamento;
    
- cálculo de comissão;
    
- regras de fidelidade.
    

---

# 5. Chain of Responsibility

Muito útil quando várias regras precisam ser executadas sequencialmente.

Por exemplo, validar uma compra:

```text
Pedido
  ↓
Cliente existe?
  ↓
Produto existe?
  ↓
Tem estoque?
  ↓
Cupom válido?
  ↓
Limite permitido?
  ↓
Pagamento autorizado?
```

Poderíamos ter:

```text
ValidarCliente
      ↓
ValidarEstoque
      ↓
ValidarCupom
      ↓
ValidarPagamento
```

Cada componente cuida de uma responsabilidade.

---

# 6. Specification

Esse padrão é excelente para regras comerciais complexas.

Imagine:

> Cliente pode ganhar frete grátis se for VIP, tiver comprado mais de R$300 e estiver em uma região atendida.

Em vez de:

```python
if cliente.vip and pedido.total > 300 and endereco.uf in estados:
```

podemos criar:

```text
ClienteVip
AND
PedidoMaiorQue300
AND
RegiaoElegivel
```

Algo conceitualmente assim:

```python
regra = (
    ClienteVip()
    & PedidoMinimo(300)
    & RegiaoElegivel()
)
```

Muito útil para:

- promoções;
    
- elegibilidade;
    
- crédito;
    
- logística;
    
- campanhas;
    
- políticas comerciais.
    

---

# 7. Factory

Eu usaria quando objetos precisam ser construídos de maneiras diferentes.

Exemplo:

```text
Pagamento
   ├── PIX
   ├── Crédito
   ├── Débito
   └── Boleto
```

Uma factory poderia decidir:

```python
processador = ProcessadorPagamentoFactory.criar(tipo_pagamento)
```

Também serviria para:

```text
Transportadora
Nota fiscal
Cliente
Pedidos
Relatórios
Integrações
```

---

# 8. Adapter

Em varejo, esse provavelmente seria um dos padrões mais importantes.

Empresas normalmente integram com:

```text
ERP
Gateway de pagamento
Transportadoras
Marketplaces
CRM
Bancos
Antifraude
Nota fiscal
Fornecedores
```

Imagine que internamente temos:

```python
class GatewayPagamento(Protocol):

    def cobrar(self, pagamento):
        ...
```

Podemos criar:

```text
AdapterMercadoPago
AdapterStone
AdapterCielo
AdapterPagSeguro
```

A regra de negócio não precisa saber como cada API funciona.

---

# 9. Facade

Integrações externas costumam ser complicadas.

Uma API de ERP pode exigir:

```text
login
↓
token
↓
cliente
↓
pedido
↓
itens
↓
imposto
↓
nota
```

Eu esconderia isso atrás de:

```python
erp.registrar_pedido(pedido)
```

A facade encapsula toda essa complexidade.

---

# 10. Anti-Corruption Layer

Especialmente importante quando há ERP legado.

Imagine um ERP retornando:

```json
{
    "CODCLI": "01922",
    "NMCLI": "JOAO",
    "STS": "A"
}
```

Mas dentro do sistema usamos:

```python
Cliente(
    codigo="01922",
    nome="João",
    ativo=True
)
```

Eu não deixaria:

```text
CODCLI
NMCLI
STS
```

se espalharem pela aplicação.

Criaria uma camada:

```text
ERP
 ↓
Anti-Corruption Layer
 ↓
Modelo interno
```

Isso protege o domínio.

---

# 11. DTO

Muito importante para APIs e integrações.

Por exemplo:

```python
@dataclass
class CriarPedidoDTO:
    cliente_id: UUID
    produtos: list[ItemPedidoDTO]
```

DTO é especialmente útil entre:

```text
HTTP
 ↓
Application
 ↓
Domain
```

e:

```text
Sistema interno
 ↓
Integração externa
```

---

# 12. Value Object

Muito útil em domínio de varejo.

Eu provavelmente criaria objetos como:

```text
Dinheiro
CPF
CNPJ
Email
Endereco
SKU
CodigoProduto
Quantidade
Percentual
```

Exemplo:

```python
@dataclass(frozen=True)
class Dinheiro:
    valor: Decimal
    moeda: str = "BRL"
```

Em vez de passar simplesmente:

```python
preco = 19.99
```

o domínio possui significado.

---

# 13. Aggregate / Aggregate Root

DDD seria útil para domínios mais importantes.

Por exemplo:

```text
Pedido
 ├── ItemPedido
 ├── EnderecoEntrega
 ├── Desconto
 └── Pagamento
```

`Pedido` poderia ser o Aggregate Root.

Isso impede coisas como:

```python
item.quantidade = -50
```

sem passar pelas regras do pedido.

Idealmente:

```python
pedido.alterar_quantidade(...)
```

---

# 14. Domain Events

Muito valioso.

Quando:

```text
PedidoCriado
```

acontece, outras partes podem reagir:

```text
PedidoCriado
      │
      ├── Reservar estoque
      ├── Calcular pontos
      ├── Enviar email
      ├── Atualizar analytics
      └── Notificar ERP
```

Isso reduz acoplamento.

---

# 15. Publish/Subscribe

Especialmente útil em uma empresa maior.

Por exemplo:

```text
PedidoPago
      │
      ├── Estoque
      ├── Logística
      ├── CRM
      ├── Fidelidade
      ├── Analytics
      └── ERP
```

O serviço de pedidos não precisa chamar diretamente seis sistemas.

Ele publica:

```text
pedido.pago
```

e os consumidores tratam o evento.

---

# 16. Transactional Outbox

Esse padrão eu consideraria extremamente importante.

Imagine:

```python
salvar_pedido()
publicar_evento()
```

O banco salva o pedido:

```text
✓
```

mas o RabbitMQ fica indisponível:

```text
✗
```

Agora o pedido existe, mas ninguém recebeu o evento.

O Outbox resolve isso:

```text
TRANSAÇÃO

Pedido
+
Evento Outbox
     ↓
COMMIT
```

Depois:

```text
Outbox Worker
     ↓
Kafka / RabbitMQ
```

Isso melhora muito a confiabilidade.

---

# 17. Idempotent Consumer

Importantíssimo para mensagens.

Suponha que recebemos duas vezes:

```text
PagamentoConfirmado #897
PagamentoConfirmado #897
```

O sistema não pode:

```text
adicionar saldo duas vezes
emitir duas notas
reservar estoque duas vezes
```

O consumidor precisa reconhecer:

```text
evento #897 já processado
```

e ignorá-lo.

---

# 18. Saga

Usaria em processos distribuídos mais complexos.

Por exemplo:

```text
Criar pedido
    ↓
Reservar estoque
    ↓
Cobrar pagamento
    ↓
Gerar nota
    ↓
Criar entrega
```

Se a cobrança falha:

```text
Reserva estoque
      ↓
Pagamento ✗
      ↓
Liberar estoque
```

Isso é uma transação distribuída baseada em compensações.

---

# 19. Circuit Breaker

Fundamental ao consumir APIs externas.

Imagine:

```text
Seu sistema
     ↓
Transportadora
```

A transportadora fica fora do ar.

Sem Circuit Breaker:

```text
request
request
request
request
request
request
```

Seu próprio sistema pode começar a travar.

Com Circuit Breaker:

```text
CLOSED
  ↓
muitas falhas
  ↓
OPEN
```

Durante um período:

```text
não chamar API externa
```

Depois:

```text
HALF OPEN
```

testa novamente.

---

# 20. Retry + Exponential Backoff + Jitter

Eu usaria nas integrações.

Em vez de:

```text
retry
1s
retry
1s
retry
1s
```

teríamos algo semelhante a:

```text
1s
2s
4s
8s
16s
```

com pequenas variações aleatórias.

Muito útil para:

- gateways;
    
- ERP;
    
- transportadoras;
    
- marketplaces;
    
- bancos.
    

---

# 21. Cache-Aside

Muito útil para catálogo.

Imagine milhões de consultas:

```text
GET /produto/123
```

Não faz sentido consultar o banco toda vez.

```text
App
 │
 ▼
Redis
 │
 ├── encontrou → retorna
 │
 └── não encontrou
          ↓
         Banco
          ↓
        Redis
```

Bom para:

- produtos;
    
- catálogo;
    
- categorias;
    
- configurações;
    
- promoções;
    
- sessões.
    

---

# 22. CQRS

Eu não aplicaria automaticamente.

Mas poderia fazer sentido para áreas com muita leitura.

Por exemplo:

```text
Catálogo

ESCRITA
Produto
Preço
Estoque

LEITURA
Página do produto
Pesquisa
Relatórios
Dashboard
```

Podemos separar:

```text
Command Model
    +
Query Model
```

Especialmente em sistemas de grande escala.

---

# 23. Observer

Para aplicações menores, poderia substituir mensageria pesada.

Por exemplo:

```text
Pedido
 ↓
PedidoObserver
   ├── Email
   ├── Estoque
   └── Auditoria
```

Se o sistema cresce, isso pode evoluir para eventos assíncronos.

---

# 24. State

Muito interessante para pedidos.

Um pedido não é simplesmente:

```python
status = "qualquer_coisa"
```

Temos transições permitidas:

```text
CRIADO
  ↓
AGUARDANDO_PAGAMENTO
  ↓
PAGO
  ↓
SEPARANDO
  ↓
ENVIADO
  ↓
ENTREGUE
```

E caminhos alternativos:

```text
AGUARDANDO_PAGAMENTO
        ↓
    CANCELADO
```

O State Pattern pode impedir:

```text
ENTREGUE → AGUARDANDO_PAGAMENTO
```

---

# 25. Command

Ótimo para casos de uso.

Em vez de:

```python
pedido_service.fazer_um_monte_de_coisas(...)
```

podemos trabalhar com:

```text
CriarPedidoCommand
CancelarPedidoCommand
ConfirmarPagamentoCommand
ReservarEstoqueCommand
```

Por exemplo:

```python
@dataclass
class CriarPedidoCommand:
    cliente_id: UUID
    itens: list[Item]
```

Isso combina muito bem com:

```text
Command
+
Handler
+
CQRS
```

---

# Minha combinação para uma empresa de varejo

Eu começaria com algo assim:

```text
                        API
                         │
                         ▼
                 Application Layer
                         │
                Command / Use Case
                         │
                         ▼
                       Domain
            ┌────────────┼────────────┐
            │            │            │
         Entity      Value Object   Strategy
            │
            ▼
        Repository
            │
            ▼
     Infrastructure
      │      │       │
      ▼      ▼       ▼
     DB     ERP    Payment
            │
          Adapter
```

Para integração:

```text
Sistema
   │
   ├── Adapter
   ├── Facade
   ├── Retry
   ├── Timeout
   ├── Circuit Breaker
   └── Anti-Corruption Layer
```

E para eventos:

```text
Pedido Pago
     │
     ▼
Transactional Outbox
     │
     ▼
Message Broker
     │
 ┌───┼────┬──────┐
 ▼   ▼    ▼      ▼
ERP Estoque CRM Logística
```

## Os 15 que eu priorizaria

Se eu chegasse amanhã para trabalhar como desenvolvedor em uma empresa de varejo, estes seriam os padrões que eu procuraria dominar primeiro:

1. **Repository**
    
2. **Unit of Work**
    
3. **Strategy**
    
4. **Adapter**
    
5. **Facade**
    
6. **Factory**
    
7. **Specification**
    
8. **Dependency Injection**
    
9. **Value Object**
    
10. **Domain Events**
    
11. **Transactional Outbox**
    
12. **Idempotency**
    
13. **Retry**
    
14. **Circuit Breaker**
    
15. **Saga**
    

E usaria **Clean/Hexagonal Architecture + DDD de forma pragmática** como estrutura geral.

O mais importante é que eu não começaria construindo `CQRS + Event Sourcing + Kafka + Microservices + DDD` para um CRUD de cadastro de fornecedores. **O padrão deve aparecer porque existe um problema que justifique sua existência.** Em varejo, principalmente, simplicidade operacional vale muito: um monólito modular bem dividido costuma ser melhor ponto de partida do que microserviços prematuros.
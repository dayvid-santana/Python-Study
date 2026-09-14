# Pacotes e `__init__.py`

## O que é

Um arquivo `__init__.py` faz uma pasta ser tratada como um **pacote Python tradicional**.

```text
meu_projeto/
├── app/
│   ├── __init__.py
│   └── calculos.py
└── main.py
```

```python
# main.py
from app.calculos import somar
```

## Por que usar

- Deixa explícito que a pasta é código Python importável.
- Permite executar configuração leve do pacote.
- Permite escolher uma API curta para quem importa o pacote.

```python
# app/__init__.py
from .calculos import somar

# outro arquivo
from app import somar
```

O ponto em `.calculos` significa “módulo dentro deste mesmo pacote”.

## Imports absolutos e relativos

Considere esta estrutura:

```text
meu_projeto/
├── app/
│   ├── __init__.py
│   ├── calculos.py
│   └── relatorios/
│       ├── __init__.py
│       └── mensal.py
└── main.py
```

Um **import absoluto** começa no nome do pacote e não depende da posição do arquivo:

```python
# main.py ou qualquer módulo do projeto
from app.calculos import somar
from app.relatorios.mensal import gerar
```

Um **import relativo** usa pontos para caminhar a partir do pacote atual:

```python
# app/relatorios/mensal.py
from ..calculos import somar  # .. sobe de relatorios para app
```

- `.`: o pacote atual.
- `..`: o pacote pai.
- `...`: sobe dois níveis.

Prefira imports **absolutos** como padrão: eles deixam a origem explícita e continuam fáceis de entender quando um arquivo muda de pasta. Use relativos dentro do pacote quando a relação local for bem evidente.

## Relação com `__init__.py`

Com `__init__.py`, `app` e `app.relatorios` são pacotes tradicionais: Python conhece a hierarquia e pode resolver `from ..calculos import somar`.

O arquivo também pode definir a porta de entrada do pacote:

```python
# app/__init__.py
from .calculos import somar  # relativo, pois está dentro de app

# main.py
from app import somar        # absoluto, visto de fora
```

Não execute `app/relatorios/mensal.py` diretamente com `python mensal.py`: imports relativos podem falhar porque o Python perde o contexto de pacote. A partir da raiz do projeto, execute `python -m app.relatorios.mensal`.

## Ele é obrigatório?

Não em todos os casos. Desde Python 3.3, uma pasta sem `__init__.py` pode formar um *namespace package*. Ainda assim, em projetos comuns, incluir o arquivo vazio é a escolha mais clara e compatível.

## Boas práticas

- Pode ficar vazio — isso é normal.
- Mantenha apenas imports simples e metadados nele.
- Não faça conexões, leitura de arquivos ou processamento pesado: ele roda na primeira importação do pacote.
- Evite importar módulos que se importam entre si; isso causa importação circular.

> Regra prática: crie `__init__.py` em cada pasta de código que você quer importar como pacote. Não precisa colocá-lo em pastas de dados, imagens ou documentação.

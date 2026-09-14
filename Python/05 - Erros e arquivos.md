# Erros e arquivos

## Exceções

```python
try:
    idade = int(texto)
except ValueError:
    idade = None
else:
    print("Conversão concluída")
finally:
    print("Sempre executa")
```

Capture exceções específicas. Não use `except:` vazio: ele esconde falhas importantes.

Para sinalizar um problema:

```python
if quantidade < 0:
    raise ValueError("quantidade não pode ser negativa")
```

## Arquivos

```python
from pathlib import Path

caminho = Path("notas.txt")
caminho.write_text("Olá\\n", encoding="utf-8")
conteudo = caminho.read_text(encoding="utf-8")
```

Para arquivos grandes, use contexto:

```python
with open("dados.txt", encoding="utf-8") as arquivo:
    for linha in arquivo:
        print(linha.rstrip())
```

## JSON

```python
import json

dados = json.loads('{"nome": "Ana"}')
texto = json.dumps(dados, ensure_ascii=False, indent=2)
```

`loads`/`dumps` trabalham com texto; `load`/`dump`, com arquivos abertos.

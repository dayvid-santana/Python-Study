# Ambiente e ferramentas

## Projeto isolado

No terminal, dentro da pasta do projeto:

```powershell
python -m venv .venv
.\\.venv\\Scripts\\Activate.ps1
python -m pip install requests
```

Use `python -m pip`, para garantir que o `pip` pertence ao Python/ambiente ativo.

## Dependências

```powershell
python -m pip freeze > requirements.txt
python -m pip install -r requirements.txt
```

Prefira registrar apenas dependências diretas quando o projeto crescer; ferramentas como `uv` ou Poetry podem ajudar nisso.

## Rodar e testar

```powershell
python programa.py
python -m unittest discover
```

Um teste simples:

```python
import unittest

class TesteSoma(unittest.TestCase):
    def test_soma(self):
        self.assertEqual(2 + 3, 5)
```

## Qualidade

- Formatação: `ruff format .`
- Lint: `ruff check .`
- Tipos: `mypy .`

Instale apenas o que for usar: `python -m pip install ruff mypy`.

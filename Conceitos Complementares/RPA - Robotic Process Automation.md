## O que é RPA?

**RPA** significa **Robotic Process Automation**, ou **Automação Robótica de Processos**.

RPA é uma abordagem usada para automatizar tarefas repetitivas que normalmente seriam realizadas manualmente por uma pessoa em sistemas digitais.

Apesar do termo **robótica**, RPA normalmente não envolve robôs físicos. O "robô" é um software responsável por executar automaticamente uma sequência de ações.

---

## Exemplo simples

Imagine que um funcionário precisa executar diariamente o seguinte processo:

1. Acessar um sistema.
    
2. Realizar login.
    
3. Abrir uma página de colaboradores.
    
4. Consultar os usuários cadastrados.
    
5. Copiar os dados encontrados.
    
6. Organizar as informações.
    
7. Salvar os dados em uma planilha ou banco de dados.
    
8. Gerar um relatório.
    

Esse processo poderia ser automatizado por um RPA.

O fluxo passaria a ser:

```text
Processo manual
      ↓
Automação
      ↓
Autenticação
      ↓
Coleta de dados
      ↓
Validação
      ↓
Transformação
      ↓
Persistência
      ↓
Relatório
```

---

# Objetivo do RPA

O principal objetivo de um RPA é reduzir a necessidade de intervenção humana em processos:

- repetitivos;
    
- previsíveis;
    
- baseados em regras;
    
- executados frequentemente;
    
- sujeitos a erros humanos;
    
- que envolvem múltiplos sistemas.
    

Um RPA pode executar essas tarefas de maneira automática e padronizada.

---

# Como um RPA funciona?

Um RPA normalmente executa uma sequência previamente definida de operações.

Por exemplo:

```text
Início
  ↓
Autenticar
  ↓
Consultar sistema
  ↓
Coletar informações
  ↓
Validar dados
  ↓
Transformar dados
  ↓
Salvar resultado
  ↓
Finalizar
```

Em código, conceitualmente poderíamos representar isso assim:

```python
def executar():
    sessao = autenticar()

    dados = coletar_dados(sessao)

    dados_validos = validar_dados(dados)

    dados_processados = processar_dados(dados_validos)

    salvar_dados(dados_processados)
```

Cada função representa uma etapa do processo automatizado.

---

# RPA não significa necessariamente automação de navegador

É comum associar RPA a ferramentas que controlam:

- mouse;
    
- teclado;
    
- navegador;
    
- aplicações desktop;
    
- formulários.
    

Por exemplo:

```text
RPA
 ↓
Abrir navegador
 ↓
Preencher usuário
 ↓
Preencher senha
 ↓
Clicar em Login
 ↓
Abrir página
 ↓
Copiar informações
```

Entretanto, essa é apenas uma das formas de implementar uma automação.

Um processo também pode ser automatizado utilizando:

- APIs;
    
- requisições HTTP;
    
- banco de dados;
    
- arquivos;
    
- filas;
    
- serviços internos.
    

---

# Automação por interface

Uma automação baseada na interface tenta reproduzir as ações realizadas por uma pessoa.

Exemplo:

```text
RPA
 ↓
Navegador
 ↓
HTML
 ↓
Botões
 ↓
Formulários
 ↓
Sistema
```

Ferramentas frequentemente utilizadas incluem:

- Selenium;
    
- Playwright;
    
- UiPath;
    
- Power Automate;
    
- Automation Anywhere.
    

Esse tipo de automação pode ser necessário quando o sistema não oferece uma interface de integração adequada.

---

# Automação por HTTP

Quando o sistema utiliza endpoints HTTP que podem ser consumidos diretamente, a automação pode evitar a interface gráfica.

O fluxo pode ser:

```text
Programa
   ↓
HTTP
   ↓
Servidor
   ↓
Resposta
   ↓
Processamento
```

Por exemplo:

```python
import requests

sessao = requests.Session()

resposta = sessao.get(
    "https://exemplo.com/dados"
)
```

Nesse modelo, a automação conversa diretamente com o servidor.

Quando disponível e autorizado, esse tipo de integração costuma ser mais estável do que automatizar cliques na interface.

---

# RPA e Web Scraping são a mesma coisa?

Não.

## Web Scraping

Web scraping é uma técnica utilizada principalmente para **extrair informações de páginas web**.

Exemplo:

```text
Página HTML
    ↓
Parser
    ↓
Extração
    ↓
Dados estruturados
```

---

## RPA

RPA representa a automação de um **processo completo**.

Por exemplo:

```text
RPA
├── autenticação
├── navegação
├── coleta de dados
├── scraping
├── validação
├── processamento
├── persistência
└── geração de relatório
```

Portanto, web scraping pode ser apenas uma etapa dentro de um RPA.

---

# RPA e integração de sistemas

RPA também não deve ser confundido automaticamente com uma integração tradicional.

Se dois sistemas possuem APIs, pode existir algo como:

```text
Sistema A
    ↓
   API
    ↓
Integração
    ↓
   API
    ↓
Sistema B
```

Nesse cenário, temos principalmente uma **integração entre sistemas**.

RPA costuma ser especialmente útil quando existe um processo humano que precisa ser automatizado, principalmente quando os sistemas envolvidos não possuem mecanismos de integração adequados.

---

# Exemplo de arquitetura

Um RPA desenvolvido em Python poderia ser organizado da seguinte maneira:

```text
rpa/
├── autenticacao/
│   ├── cliente_http.py
│   └── credenciais.py
│
├── coleta/
│   ├── coletor.py
│   └── parser.py
│
├── dominio/
│   ├── usuario.py
│   └── empresa.py
│
├── infraestrutura/
│   ├── banco.py
│   └── repositorio.py
│
├── processamento/
│   ├── validacao.py
│   └── transformacao.py
│
└── main.py
```

O fluxo poderia ser:

```text
main.py
   ↓
Autenticação
   ↓
Coleta
   ↓
Parser
   ↓
Validação
   ↓
Transformação
   ↓
Repositório
   ↓
Banco de dados
```

---

# Exemplo de processo empresarial

Considere uma empresa que precisa verificar quais colaboradores possuem acesso a determinado sistema administrativo.

Atualmente o processo pode ser:

```text
Funcionário
   ↓
Acessa sistema
   ↓
Realiza login
   ↓
Abre listagem
   ↓
Consulta colaboradores
   ↓
Copia dados
   ↓
Atualiza controle interno
```

Com automação:

```text
Servidor
   ↓
RPA
   ↓
Autenticação
   ↓
Consulta
   ↓
Coleta
   ↓
Validação
   ↓
Banco de dados
```

O processo deixa de depender da execução manual de uma pessoa.

---

# Características de um bom candidato a RPA

Um processo costuma ser um bom candidato a automação quando possui:

- muitas tarefas repetitivas;
    
- regras bem definidas;
    
- grande volume de execução;
    
- baixa necessidade de julgamento humano;
    
- entradas previsíveis;
    
- resultados previsíveis;
    
- grande quantidade de trabalho manual;
    
- risco frequente de erro humano.
    

---

# Benefícios do RPA

Entre os principais benefícios estão:

## Redução de trabalho manual

Processos repetitivos deixam de consumir tempo dos colaboradores.

## Padronização

O processo é executado sempre seguindo as mesmas regras.

## Redução de erros

Erros de digitação, esquecimento ou execução podem ser reduzidos.

## Velocidade

Sistemas conseguem executar determinadas tarefas muito mais rapidamente que uma pessoa.

## Rastreabilidade

A automação pode registrar:

- horário da execução;
    
- dados processados;
    
- quantidade de registros;
    
- falhas;
    
- exceções;
    
- resultados.
    

## Escalabilidade

Uma automação pode processar grandes volumes de dados sem aumentar proporcionalmente o esforço manual.

---

# Limitações

RPA também possui limitações.

Automatizações baseadas em interfaces podem ser frágeis.

Por exemplo, uma mudança de:

```text
id="botao-login"
```

para:

```text
id="login-button"
```

pode quebrar uma automação que depende daquele elemento.

Outros problemas podem incluir:

- mudanças de layout;
    
- CAPTCHA;
    
- autenticação multifator;
    
- alterações no fluxo da aplicação;
    
- bloqueios de automação;
    
- expiração de sessão;
    
- mudanças na estrutura HTML;
    
- indisponibilidade do sistema.
    

---

# Importância dos logs

Um RPA deve possuir boa observabilidade.

Por exemplo:

```text
2026-09-10 08:00:00 - Execução iniciada
2026-09-10 08:00:01 - Autenticação realizada
2026-09-10 08:00:03 - 237 registros encontrados
2026-09-10 08:00:04 - 237 registros validados
2026-09-10 08:00:05 - Dados persistidos
2026-09-10 08:00:05 - Execução concluída
```

Se ocorrer uma falha:

```text
2026-09-10 08:00:03 - ERRO - Sessão expirada
```

Isso facilita investigação e manutenção.

---

# Tratamento de erros

Automação não deve simplesmente falhar silenciosamente.

É importante prever situações como:

```text
Autenticação falhou
        ↓
registrar erro
        ↓
interromper execução
```

ou:

```text
Dados incompletos
      ↓
não persistir
      ↓
registrar problema
      ↓
gerar alerta
```

Isso evita que dados incorretos sejam propagados.

---

# Segurança

RPAs frequentemente trabalham com informações sensíveis.

Por isso, credenciais não devem ser colocadas diretamente no código.

Evite:

```python
usuario = "admin"
senha = "123456"
```

Prefira mecanismos como:

```text
Variáveis de ambiente
Secrets Manager
Vault
Serviços de credenciais
```

Por exemplo:

```python
import os

usuario = os.environ["SISTEMA_USERNAME"]
senha = os.environ["SISTEMA_PASSWORD"]
```

Também é importante proteger:

- cookies;
    
- tokens;
    
- sessões;
    
- informações pessoais;
    
- logs;
    
- credenciais;
    
- arquivos temporários.
    

---

# RPA e agendamento

É comum executar RPAs automaticamente através de:

- cron;
    
- Airflow;
    
- Celery;
    
- schedulers;
    
- servidores;
    
- pipelines;
    
- ferramentas de orquestração.
    

Por exemplo:

```text
Scheduler
    ↓
08:00
    ↓
Executar RPA
    ↓
Coletar dados
    ↓
Atualizar banco
```

---

# RPA como processo

A melhor maneira de compreender RPA não é pensar apenas em:

> "um robô que clica em botões".

Uma definição mais completa seria:

> **RPA é a automação de processos repetitivos e baseados em regras, normalmente executados por pessoas através de sistemas digitais.**

Um RPA pode utilizar diferentes mecanismos:

```text
RPA
├── HTTP
├── APIs
├── navegador
├── scraping
├── arquivos
├── banco de dados
├── aplicações desktop
└── filas
```

O mecanismo utilizado é secundário.

O principal elemento é o **processo que está sendo automatizado**.

---

# Resumo

```text
RPA
│
├── Automatiza processos
│
├── Reduz tarefas manuais
│
├── Executa regras automaticamente
│
├── Pode trabalhar com interfaces
│
├── Pode trabalhar com HTTP/APIs
│
├── Pode utilizar scraping
│
├── Pode acessar bancos e arquivos
│
├── Pode ser agendado
│
└── Deve possuir segurança, logs e tratamento de erros
```

Em resumo:

> **RPA é uma forma de transformar um processo operacional manual, repetitivo e baseado em regras em um fluxo automatizado executado por software.**
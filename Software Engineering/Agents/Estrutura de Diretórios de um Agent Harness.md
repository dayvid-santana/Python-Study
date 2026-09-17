---
tipo: arquitetura
tags: [agents-ia, llm, agent-harness, arquitetura, desenvolvimento]
aliases: [Estrutura de diretórios de Agent Harness]
---

# Estrutura de Diretórios de um Agent Harness

> [!summary]
> Um **Agent Harness** é a camada de execução que torna um ou mais [[Agent|Agents]] de IA utilizáveis com segurança e repetibilidade: carrega instruções, seleciona modelos, monta contexto, controla ferramentas, executa fluxos, preserva estado e registra resultados. A árvore desta nota é uma referência arquitetural, **não um padrão oficial nem uma estrutura obrigatória**.

## O que é um Agent Harness?

Um [[Agent]] é uma unidade de comportamento orientada a um objetivo: por exemplo, analisar uma arquitetura, implementar uma alteração ou revisar um *diff*. Em geral, ele combina instruções, um modelo de linguagem, contexto e acesso controlado a [[Tool|Tools]].

O **Harness** é a infraestrutura e as regras que fazem esse Agent operar. Ele resolve problemas que não pertencem a um papel especializado: roteamento da solicitação, ciclo de vida da execução, limites de tokens, permissões, isolamento, retomada de sessão, telemetria, testes e integração com provedores de LLM.

```text
Agent = "qual trabalho/decisão este papel executa?"
Harness = "como esse trabalho é preparado, governado, executado e observado?"
```

Sem Harness, é comum concentrar em um único prompt regras de negócio, chamadas de ferramentas, controle de erros e estado da conversa. Isso cria código difícil de testar, permissões implícitas e comportamento pouco reproduzível. Um Harness separa essas preocupações, mas não precisa nascer grande.

> [!important]
> Diretórios são uma ferramenta de modularidade, não uma meta. Só separe algo quando houver uma responsabilidade estável, regras de mudança diferentes ou necessidade real de reutilização/teste. Uma pasta vazia ou um arquivo de configuração para cada abstração é custo, não arquitetura.

## Convenções, recomendações e requisitos reais

Não existe uma árvore universal para Harnesses: frameworks, linguagens, modelo de implantação e risco operacional mudam a forma do repositório. As categorias abaixo comunicam prioridade arquitetural, não uma certificação.

| Categoria | Significado |
| --- | --- |
| **Fundamental** | Responsabilidade que todo Harness funcional precisa ter, ainda que fique em poucos arquivos ou dentro de outro diretório em um protótipo. |
| **Recomendado** | Separação que costuma compensar assim que há mais de um papel, ferramenta ou ambiente. |
| **Opcional** | Resolve uma necessidade específica; não deve ser criado antecipadamente. |
| **Avançado** | Normalmente aparece com escala, múltiplos provedores, autonomia maior, requisitos de auditoria ou avaliação contínua. |

Um requisito real é o que o produto e o ambiente impõem — por exemplo, *logs* de auditoria se há operações sensíveis, ou isolamento se o Agent executa código não confiável. Já nomes como `core/`, `providers/` ou `memory/` são **convenções úteis**, não requisitos por si só.

## Estrutura de referência completa

Esta é uma estrutura possível para um Harness de desenvolvimento de software maduro. Ela inclui diretórios que um projeto inicial provavelmente ainda não precisa.

```text
dev-harness/
├── README.md                 # propósito, instalação, operação e limites do Harness
├── AGENTS.md                 # instruções de contribuição e regras locais para Agents
├── dev-agent.yaml            # composição declarativa de Agents, Tools e limites
├── pyproject.toml            # dependências, scripts, qualidade e empacotamento Python
├── .env.example              # nomes de variáveis, nunca segredos reais
├── core/                     # runtime: orquestração, roteamento, execução e ciclo de vida
│   ├── orchestrator/
│   ├── router/
│   ├── executor/
│   ├── lifecycle/
│   ├── context/
│   └── token_manager/
├── agents/                   # definições dos papéis principais
├── subagents/                # papéis delegáveis e de escopo curto
├── skills/                   # capacidades reutilizáveis, com instruções e recursos
├── prompts/                  # blocos de prompt, mensagens de sistema e exemplos
├── tools/                    # contratos e implementações de ações externas
├── workflows/                # processos multi-etapa e grafos de execução
├── config/                   # configuração versionada por ambiente e feature flags
├── policies/                 # autorização, segurança, custo e regras de operação
├── memory/                   # conhecimento persistente por projeto ou usuário
├── context/                  # fontes, seleção e compactação de contexto externo ao runtime
├── hooks/                    # extensões em eventos do ciclo de vida
├── adapters/                 # tradução entre portas internas e sistemas concretos
├── providers/                # clientes/configurações de modelos e embeddings
├── evals/                    # cenários, datasets, métricas e *graders* de qualidade
├── schemas/                  # contratos estruturados e validação de I/O
├── templates/                # esqueletos reutilizáveis de tarefas e artefatos
├── sessions/                 # checkpoints e metadados de execuções retomáveis
├── logs/                     # saída operacional local; geralmente ignorada pelo Git
├── cache/                    # artefatos regeneráveis; geralmente ignorado pelo Git
├── sandbox/                  # perfis e imagens de isolamento para execução de código
├── tests/                    # testes do Harness, não apenas código gerado pelo Agent
└── docs/                     # decisões, guias operacionais e arquitetura do produto
```

`README.md`, `AGENTS.md`, `dev-agent.yaml`, `pyproject.toml` e `.env.example` são arquivos exemplares, não uma lista obrigatória. Em TypeScript, por exemplo, `pyproject.toml` pode ser `package.json`; uma aplicação configurada em código pode nem ter `dev-agent.yaml`.

## Classificação rápida

| Diretório | Categoria | Responsabilidade |
| --- | --- | --- |
| `core/` | Fundamental | Executar o loop do Agent e coordenar decisões de runtime. |
| `agents/` | Fundamental | Declarar papéis, objetivos e capacidades permitidas. |
| `prompts/` | Fundamental/Recomendado | Versionar instruções que mudam o comportamento do modelo. |
| `tools/` | Fundamental se há ações | Expor operações externas por contratos controlados. |
| `config/` | Recomendado | Separar parâmetros de ambiente da lógica. |
| `schemas/` | Recomendado | Validar entradas, saídas e chamadas de ferramentas. |
| `tests/` | Recomendado | Proteger comportamento determinístico e integrações. |
| `docs/` | Recomendado | Tornar operação e decisões compreensíveis. |
| `skills/` | Recomendado | Reutilizar conhecimento procedimental entre Agents. |
| `context/` | Recomendado quando há muitas fontes | Organizar seleção, orçamento e proveniência de contexto. |
| `workflows/` | Opcional | Modelar sequências explícitas entre passos e Agents. |
| `subagents/` | Opcional | Isolar delegações com objetivo e orçamento próprios. |
| `policies/` | Opcional/Avançado | Centralizar decisões de segurança, custo e autorização. |
| `hooks/` | Opcional | Personalizar eventos sem acoplar extensões ao runtime. |
| `providers/` | Opcional | Isolar LLMs/embeddings concretos; recomendado com mais de um. |
| `adapters/` | Opcional | Implementar integrações atrás de interfaces internas. |
| `memory/` | Opcional | Persistir conhecimento útil além de uma sessão. |
| `sessions/` | Opcional | Retomar execuções e manter histórico operacional. |
| `templates/` | Opcional | Padronizar artefatos e tarefas repetitivas. |
| `logs/` | Opcional/Avançado | Armazenar telemetria local e trilhas de auditoria. |
| `cache/` | Opcional | Evitar recomputação de resultados regeneráveis. |
| `sandbox/` | Avançado (ou obrigatório por risco) | Isolar comandos, código e arquivos não confiáveis. |
| `evals/` | Avançado | Medir qualidade, regressões e segurança de forma sistemática. |

`agents/`, `prompts/` e `skills/` podem começar juntos em um único módulo num Harness pequeno. A separação torna-se valiosa quando instruções e recursos passam a ser compartilhados, revisados ou versionados independentemente do código de execução.

## Vocabulário: o que não deve ser confundido

| Conceito | É | Não é |
| --- | --- | --- |
| [[Agent]] | Papel com objetivo, instruções, contexto e capacidades. | Um modelo de LLM ou uma simples função. |
| [[SubAgent]] | Agent delegado por outro para uma subtarefa isolada. | Apenas uma função auxiliar; tem orçamento, contexto e resultado próprios. |
| [[Skill]] | Pacote reutilizável de conhecimento procedimental: instruções, recursos, scripts e critérios. | Uma ferramenta que executa uma ação externa. |
| [[Tool]] | Interface controlada para executar uma ação: ler arquivo, rodar teste, criar *commit*. | A decisão de quando ou por que executar a ação. |
| [[Prompt Engineering|Prompt]] | Texto/estrutura de mensagens que orienta o modelo. | A identidade completa de um Agent nem uma política executável. |
| [[Workflow]] | Sequência ou grafo explícito de etapas, com estados e transições. | Um Agent improvisando seu próximo passo no loop. |
| Hook | Ponto de extensão disparado por evento do runtime. | Um workflow completo ou regra de autorização. |
| Policy | Regra verificável que permite, bloqueia ou exige aprovação. | Uma sugestão opcional no prompt. |
| Provider | Serviço/família concreta que fornece LLM, embeddings ou *reranking*. | A interface estável usada pelo domínio. |
| Adapter | Implementação que traduz uma interface interna para uma tecnologia externa. | O domínio ou a decisão de negócio. |

Uma regra prática: **Agent decide**, **Skill ensina o procedimento**, **Tool executa uma capacidade**, **Workflow ordena etapas**, **Policy limita o permitido**, e o **Harness coordena tudo**.

## Relações entre os componentes

```text
Usuário / CI / API
        │ solicitação
        ▼
     Harness
        │
        ▼
  Orchestrator ────── consulta ─────► Policies
        │                                  │
        ▼                                  ▼
      Router ───────── seleciona ───────► Agent
                                              │ usa
                                              ▼
                                        Skill + Prompt
                                              │ solicita
                                              ▼
                                            Tool
                                              │ via
                                              ▼
                                  Adapter / Provider / Sandbox
                                              │
                                              ▼
                                Filesystem, Git, testes, LLM, APIs
```

Um [[Workflow]] é adequado quando a ordem deve ser explícita e auditável:

```text
Issue aceita
    │
    ▼
ArchitectureAgent ──► plano aprovado ──► CodingAgent
                                             │
                                             ▼
                                        TestAgent
                                             │
                            falha ───────────┴──────────► CodingAgent
                                             │ sucesso
                                             ▼
                                        ReviewAgent ──► desenvolvedor
```

O Harness deve manter o acoplamento unidirecional: `core` conhece contratos de Agents e Tools; um Agent não deve importar a implementação concreta de Git, Docker ou um provedor de LLM. Ele pede capacidades por uma interface.

## Diretórios fundamentais

### `core/`

**Categoria:** Fundamental.

**Responsabilidade:** conter o runtime do Harness, isto é, os componentes que transformam uma solicitação em uma execução governada. Ele não deve guardar a personalidade de cada Agent nem integrações concretas.

**Por que existe:** evita que a orquestração fique espalhada por Agents, prompts e ferramentas. Isso melhora testabilidade, troca de agentes e consistência de políticas.

```text
core/
├── orchestrator/       # coordena plano, passos, delegações e resultado final
├── router/             # escolhe Agent, workflow ou estratégia de execução
├── executor/           # roda turnos do modelo e invoca Tools com validação
├── lifecycle/          # inicia, cancela, recupera e finaliza execuções
├── context/            # monta a janela de contexto usada no turno atual
└── token_manager/      # calcula orçamento, reserva e compacta contexto
```

#### Orchestrator

Recebe o objetivo, cria o plano de alto nível, coordena chamadas do [[Agent]], delega [[SubAgent|SubAgents]] quando necessário e consolida a resposta. Não deve codificar a regra de cada domínio: ele compõe capacidades.

Crie uma separação própria quando houver múltiplas etapas, aprovações, delegação ou necessidade de rastrear estado. Em um protótipo de único Agent, uma função `run_agent()` pode exercer esse papel.

#### Router

Decide *quem* ou *qual fluxo* atende a solicitação: por regras, classificação, custo/latência, tipo de arquivo ou decisão humana. O Router reduz prompts gigantes que tentam conter todos os papéis.

Separe-o quando a seleção variar. Não crie um Router sofisticado se só existe um Agent; a escolha nesse caso é constante.

#### Executor

Implementa o loop técnico: chama o modelo, interpreta pedidos de Tool, valida argumentos contra [[schemas/]], obtém resultados, trata *timeouts* e devolve o estado atualizado. Ele protege o sistema de formatos inválidos e falhas transitórias.

O Executor é diferente do Orchestrator: o primeiro executa um passo/turno com confiabilidade; o segundo decide a sequência de passos.

#### Lifecycle

Controla transições como `created → running → waiting_approval → completed/failed/cancelled`, cancelamento, retomada e limpeza de recursos. É especialmente importante para tarefas longas, assíncronas ou com aprovação humana.

Sem execução longa, pode ficar junto do Executor. Extraia quando o estado precisar sobreviver a um processo ou fila.

#### Gerenciamento de contexto

Seleciona arquivos, resultados de Tools, histórico, instruções e memória que realmente ajudam no turno atual; atribui origem e precedência. O problema resolvido é o contexto excessivo, desatualizado ou contraditório, que degrada custo e precisão.

O código de montagem pode começar em `core/context/`; use o diretório de topo `context/` quando também existirem índices, *retrievers*, resumos e regras editoriais fora do runtime.

#### Gerenciamento de tokens

Mantém um orçamento por solicitação, Agent, SubAgent e Tool; reserva espaço para saída; escolhe o que resumir ou descartar; registra consumo e encerra/degrada o fluxo de modo explícito. Limite de tokens é controle operacional, não apenas otimização de custo.

Em um início simples, uma configuração `max_tokens` no Agent basta. Extraia `token_manager/` quando houver contexto grande, delegações ou cobrança/limites por usuário.

### `agents/`

**Categoria:** Fundamental.

**Responsabilidade:** armazenar definições dos Agents principais e seu contrato: objetivo, entradas/saídas, instruções, Skills permitidas, Tools autorizadas, modelo preferido e limites.

**Por que existe:** separa comportamento especializado da infraestrutura do [[Orchestrator]]. Cada Agent ganha alta coesão e pode ser testado/avaliado sem carregar responsabilidades de outros papéis.

```text
agents/
├── coding/             # implementa mudanças e testes associados
├── testing/            # diagnostica falhas e propõe/roda testes
├── architecture/       # analisa limites, contratos e trade-offs
├── review/             # avalia regressão, segurança e manutenção
└── git/                # prepara mudanças versionáveis sob política
```

**Exemplos:** `CodingAgent`, `TestAgent`, `ArchitectureAgent`, `ReviewAgent` e `GitAgent`.

**Relacionamento:** o Orchestrator seleciona o Agent; o Agent aplica Skills e Prompts; o Executor oferece apenas as Tools permitidas.

**Quando criar:** sempre que houver identidade ou objetivo de Agent. Mesmo um único Agent merece um contrato claro, embora possa ficar no módulo principal no começo.

**Quando não separar:** não transforme cada etapa banal em Agent. Se "formatar código" é só uma chamada de Tool determinística, mantenha-a como Tool ou Skill, não como outro papel com LLM.

### `prompts/`

**Categoria:** Fundamental/Recomendado.

**Responsabilidade:** versionar mensagens de sistema, instruções de tarefa, exemplos, formatos de saída e fragmentos reutilizáveis de [[Prompt Engineering]].

**Por que existe:** prompts são parte do comportamento de produção. Fora do código ou misturados em strings, ficam difíceis de revisar, comparar e avaliar.

```text
prompts/
├── system/             # limites universais e estilo do Harness
├── agents/             # instruções específicas por papel
├── fragments/          # blocos compostos pelo runtime
└── examples/           # exemplos curados de entrada e saída
```

**Quando criar:** assim que prompts tiverem tamanho, reutilização ou ciclo de revisão independente. Em um experimento curto, a definição do Agent pode conter seu prompt.

**Cuidado:** o prompt descreve comportamento esperado, mas não substitui validação de schema, sandbox ou Policy. "Não execute comandos destrutivos" em texto não é controle de segurança suficiente.

### `tools/`

**Categoria:** Fundamental se o Agent executa ações; recomendado nos demais casos.

**Responsabilidade:** declarar e implementar ações com efeito ou acesso externo, como leitura/escrita de arquivos, execução de testes, Git, busca de código e chamadas HTTP.

**Por que existe:** centraliza validação de argumentos, permissões, observabilidade, tratamento de erro e contratos. Evita conceder ao Agent acesso arbitrário a uma biblioteca/shell.

```text
tools/
├── filesystem/         # read_file, list_files, patch_file com escopo permitido
├── shell/              # executor de comandos com limites e listas de permissão
├── git/                # status, diff, branch, commit sob políticas
├── testing/            # rodar testes e retornar saída estruturada
├── code_search/        # índice, ripgrep ou servidor de linguagem
└── web/                # clientes HTTP/busca, se autorizados
```

**Quando criar:** ao introduzir qualquer ação além de gerar texto. Crie Tools pequenas, com entradas e saídas tipadas, idempotência quando possível e efeito declarado.

**Quando não criar uma Tool:** não envolva uma função puramente interna e trivial em uma interface de Tool só para aumentar a árvore. Tools devem representar uma capacidade que precisa ser governada ou chamada pelo modelo.

## Diretórios recomendados

### `skills/`

**Categoria:** Recomendado; pode ser fundamental em plataformas onde Skill é a unidade de extensão.

**Responsabilidade:** agrupar conhecimento procedimental reutilizável para uma família de tarefas. Uma Skill pode conter `SKILL.md`, instruções, exemplos, scripts seguros, checklists e referências.

**Por que existe:** evita duplicar instruções de procedimento em vários Agents e permite melhorar um método sem editar todos os papéis.

```text
skills/
├── code-review/
│   ├── SKILL.md
│   ├── checklist.md
│   └── examples/
├── pytest/
│   ├── SKILL.md
│   └── scripts/
└── migration-safety/
    ├── SKILL.md
    └── references/
```

**Quando criar:** quando dois Agents usam o mesmo procedimento, ou uma tarefa requer instrução rica e recursos anexos. Não crie Skill para uma única frase de prompt ou para encapsular uma Tool.

### `config/`

**Categoria:** Recomendado.

**Responsabilidade:** manter configuração não secreta e versionável por ambiente: modelos padrão, orçamentos, *timeouts*, habilitação de Tools, caminhos de workspace e *feature flags*.

**Por que existe:** permite alterar operação sem espalhar constantes no runtime e deixa diferenças de ambiente explícitas.

```text
config/
├── base.yaml
├── development.yaml
├── ci.yaml
└── production.yaml
```

**Quando criar:** quando há mais de um ambiente, configuração suficiente para poluir o código ou necessidade de auditoria de parâmetros. Segredos pertencem ao gerenciador de segredos/variáveis de ambiente, documentados apenas por `.env.example`.

### `schemas/`

**Categoria:** Recomendado.

**Responsabilidade:** definir contratos de dados para solicitações, planos, mensagens, chamadas de Tools, resultados, eventos e configurações.

**Por que existe:** LLMs e integrações produzem dados incertos; schemas tornam falhas explícitas na fronteira e evitam que formato implícito se propague.

```text
schemas/
├── agent.py            # AgentDefinition, AgentResult
├── tool.py             # ToolCall, ToolResult
├── workflow.py         # WorkflowState, StepResult
└── events.py           # eventos para logs e hooks
```

**Quando criar:** ao usar saída estruturada, múltiplas Tools ou comunicação entre módulos. Em um código pequeno, os modelos/schema podem ficar perto de `core/`; extraia quando virarem contratos transversais.

### `tests/`

**Categoria:** Recomendado.

**Responsabilidade:** testar o Harness: regras de roteamento, autorização, validação, montagem de contexto, Tools, adapters e fluxos previsíveis.

**Por que existe:** reduz regressões introduzidas por mudanças em prompt, modelo, configuração ou integração. A qualidade do código gerado pelo Agent é apenas uma parte da qualidade do Harness.

```text
tests/
├── unit/               # roteador, orçamento, validação e políticas
├── integration/        # Tools, provider simulado e sandbox
├── contract/           # aderência de adapters e providers às interfaces
└── fixtures/           # repositórios e respostas controladas para teste
```

**Quando criar:** desde cedo. Não faça testes de integração dependentes de uma LLM real serem o único sinal de qualidade: use *fakes* e fixtures para testar decisões determinísticas.

### `docs/`

**Categoria:** Recomendado.

**Responsabilidade:** documentação de operação e decisões do próprio projeto: arquitetura, ameaça, runbooks, convenções de Tool e guias de contribuição.

**Por que existe:** Harnesses misturam comportamento probabilístico e efeitos reais; documentação reduz ambiguidade para mantenedores e operadores.

**Quando criar:** se o `README.md` já não comporta explicações sem perder função de entrada. Esta nota de vault pode ficar fora do repositório do Harness; `docs/` contém a documentação distribuída com ele.

### `context/`

**Categoria:** Recomendado quando o contexto vem de fontes múltiplas.

**Responsabilidade:** organizar artefatos de [[Context Engineering]] fora do loop do runtime: políticas de seleção, índices, *retrievers*, resumos, manifestos de repositório e metadados de proveniência.

**Por que existe:** evita enviar indiscriminadamente o repositório ou o histórico inteiro ao modelo, preservando relevância, custo e rastreabilidade.

**Quando criar:** quando a montagem de contexto inclui RAG, índices, documentação extensa ou regras de compactação. Se só há a solicitação atual e um `AGENTS.md`, mantenha a lógica em `core/context/`.

## Diretórios opcionais e avançados

### `subagents/`

**Categoria:** Opcional.

**Responsabilidade:** definir Agents delegáveis, de escopo estreito e vida curta — por exemplo, investigar uma falha, mapear impacto ou pesquisar uma API.

**Por que existe:** delegação limita o contexto de cada subtarefa, permite paralelismo controlado e devolve resultados estruturados ao Agent pai.

```text
subagents/
├── repository-explorer/
├── test-failure-investigator/
└── security-diff-scanner/
```

**Quando criar:** quando o trabalho contém subtarefas independentes e o benefício da especialização supera custo, latência e coordenação. Cada SubAgent deve ter entrada, saída, limite de tempo/tokens e autoridade explícitos.

**Quando evitar:** em tarefas lineares ou pequenas. Delegar sem necessidade cria perda de contexto, duplicação de pesquisa e resultados conflitantes. Um SubAgent pode ficar em `agents/` em um Harness pequeno; separe quando seu ciclo de vida de delegação for distinto.

### `workflows/`

**Categoria:** Opcional.

**Responsabilidade:** armazenar processos declarativos ou código de grafos que definem ordem, condições de transição, repetição, aprovação e compensação.

**Por que existe:** torna previsível uma cadeia que não deveria depender apenas da improvisação de um modelo, como implementar → testar → revisar → pedir aprovação.

```text
workflows/
├── feature_delivery.yaml
├── incident_triage.yaml
└── dependency_upgrade.yaml
```

**Quando criar:** quando há sequência estável, etapas condicionais, handoff entre Agents ou requisito de auditoria. Não use Workflow só para encapsular uma chamada de Agent; um método bem nomeado basta.

### `policies/`

**Categoria:** Opcional; Avançado em ambientes com risco/compliance.

**Responsabilidade:** codificar decisões de segurança e operação: quais Tools e argumentos são permitidos, quais caminhos são graváveis, quando exigir aprovação, limites de custo e regras de retenção.

**Por que existe:** segurança não pode depender da boa vontade do modelo ou de texto em prompt. Policies devem ser verificadas pelo Executor antes do efeito.

```text
policies/
├── tool_access.yaml
├── filesystem.yaml
├── git.yaml
├── approval_rules.yaml
└── data_retention.yaml
```

**Quando criar:** antes de habilitar escrita, shell, rede, Git remoto ou dados sensíveis. Em um projeto mínimo, a Policy pode ser código junto da Tool; centralize ao aparecerem regras reutilizadas ou auditadas.

### `hooks/`

**Categoria:** Opcional.

**Responsabilidade:** extensões acionadas por eventos previsíveis, como `before_tool_call`, `after_tool_call`, `before_agent_run`, `on_error` e `after_workflow`.

**Por que existe:** permite adicionar telemetria, mascaramento de segredos, validações ou notificações sem modificar Orchestrator/Executor.

**Quando criar:** quando extensões são plugáveis e têm ciclo de mudança independente. Mantenha Hooks curtos, determinísticos e com contrato de erro; lógica principal de negócio não deve esconder-se em Hooks.

### `adapters/`

**Categoria:** Opcional.

**Responsabilidade:** implementar as portas internas para tecnologias concretas: cliente Git, executor Docker, banco vetorial, fila, filesystem local ou API corporativa.

**Por que existe:** protege `core` e Agents contra dependência direta de fornecedores. Um Adapter pode ser trocado e coberto por testes de contrato.

```text
adapters/
├── git_cli/
├── docker_executor/
├── local_filesystem/
└── postgres_memory/
```

**Quando criar:** quando há implementação externa substituível, dois ambientes diferentes ou teste com *fake*. Em um protótipo de tecnologia única, pode residir dentro da Tool correspondente.

### `providers/`

**Categoria:** Opcional; recomendado com múltiplos modelos.

**Responsabilidade:** encapsular SDKs e especificidades de provedores de LLM, embeddings, *rerankers* ou moderação, incluindo autenticação, limites, *retries* e capacidade suportada.

**Por que existe:** o restante do Harness pede uma capacidade (`generate`, `embed`) e não conhece parâmetros proprietários do fornecedor.

```text
providers/
├── openai/
├── anthropic/
├── local_model/
└── embeddings/
```

**Provider x Adapter:** em uma leitura ampla, o Provider pode ser um tipo de Adapter. A separação é útil quando "provedor de inteligência" é uma preocupação de produto independente das integrações operacionais. Em estrutura pequena, use apenas `adapters/` ou apenas `providers/`, não ambos sem motivo.

### `memory/`

**Categoria:** Opcional.

**Responsabilidade:** guardar [[Agent Memory|memória]] persistente e recuperável: convenções de projeto, decisões já confirmadas, preferências do usuário, resumos de sessões e fatos com fonte/expiração.

**Por que existe:** a janela de contexto não é memória confiável nem persistente. A separação permite retenção, invalidação, proveniência e controle de privacidade.

```text
memory/
├── project/            # fatos e decisões do repositório
├── user/               # preferências autorizadas e escopadas
├── summaries/          # resumos verificáveis de sessões
└── retrieval/          # índices, políticas de busca e filtros
```

**Quando criar:** quando o Agent precisa aprender entre sessões ou o contexto não cabe de modo eficiente. Não armazene toda conversa como "memória"; registre apenas informação útil, com fonte, escopo, validade e mecanismo de correção.

### `sessions/`

**Categoria:** Opcional.

**Responsabilidade:** persistir estado operacional de execuções: identificador, etapa corrente, plano, aprovações, referências a artefatos e checkpoint.

**Por que existe:** possibilita retomar uma tarefa longa, investigar falhas e diferenciar memória durável de estado transitório.

**Quando criar:** com jobs assíncronos, filas, interface de aprovação ou retomada após reinício. Para um comando síncrono de curta duração, mantenha estado em memória e logs.

### `templates/`

**Categoria:** Opcional.

**Responsabilidade:** conter modelos de planos, ADRs, relatórios de revisão, issues, prompts de tarefa ou configurações de novos Agents.

**Por que existe:** padroniza saídas repetidas sem misturar artefatos estáticos a regras de orquestração.

**Quando criar:** quando o mesmo formato é gerado/revisado repetidamente. Um exemplo puramente didático pode ficar em `docs/` ou `prompts/examples/`.

### `logs/`

**Categoria:** Opcional/Avançado.

**Responsabilidade:** manter saída operacional local, correlação de eventos, decisões de roteamento, chamadas de Tool, consumo e falhas — com redigimento de dados sensíveis.

**Por que existe:** respostas finais não bastam para depurar um loop que usou modelos e ferramentas. Logs permitem observabilidade, suporte e auditoria.

**Quando criar:** em desenvolvimento local pode ser útil; em produção, frequentemente os logs vão para um serviço central, e a pasta é apenas um *sink* temporário ou nem existe. Não versione logs nem prompts que possam conter segredos.

### `cache/`

**Categoria:** Opcional.

**Responsabilidade:** guardar resultados regeneráveis, como embeddings, respostas de busca, árvores de arquivos ou artefatos de compilação.

**Por que existe:** reduz latência, custo e chamadas repetidas.

**Quando criar:** quando medição mostrar recomputação relevante. Defina chave, TTL, invalidação e escopo por projeto/usuário; cache sem invalidação pode dar ao Agent um retrato obsoleto do código. Normalmente é ignorado pelo Git.

### `sandbox/`

**Categoria:** Avançado — ou requisito real quando há execução não confiável.

**Responsabilidade:** declarar e implementar isolamento de shell, testes e código: imagem de contêiner, perfis de rede, mounts permitidos, limites de CPU/memória/tempo e usuários sem privilégio.

**Por que existe:** um Agent que executa comandos ou código produzido por LLM pode apagar dados, vazar credenciais, exfiltrar informações ou esgotar recursos. Prompt e Policy são complementos; sandbox é a barreira técnica.

```text
sandbox/
├── Dockerfile
├── profiles/
│   ├── read_only.yaml
│   └── test_runner.yaml
└── allowlists/
```

**Quando criar:** antes de permitir shell, escrita ampla, instalação de dependências, rede ou repositórios não confiáveis. Se a execução já ocorre em plataforma isolada, o diretório pode conter só a configuração dessa plataforma ou não existir.

### `evals/`

**Categoria:** Avançado.

**Responsabilidade:** definir casos de avaliação, datasets, resultados esperados, métricas, *graders* determinísticos e revisões humanas para Agents e fluxos.

**Por que existe:** testes tradicionais validam código; [[evals]] medem qualidade comportamental e detectam regressão por mudança de prompt, modelo, contexto ou política.

```text
evals/
├── datasets/           # issues, repositórios e casos anonimizados
├── cases/              # entrada + critérios por cenário
├── graders/            # validações determinísticas ou judge model controlado
├── baselines/          # métricas aprovadas para comparação
└── reports/            # saída gerada; pode ser ignorada pelo Git
```

**Quando criar:** antes de trocar modelos/prompt em produção com frequência, ou quando autonomia e custo justificarem medição contínua. Comece por tarefas representativas e métricas objetivas (testes passam, schema válido, Tool proibida não chamada); não dependa apenas de "parece bom".

## Arquivos de raiz e fronteiras importantes

| Arquivo | Papel | Observação |
| --- | --- | --- |
| `README.md` | Entrada humana: objetivo, instalação, arquitetura curta e operação. | Não deve repetir toda a documentação interna. |
| `AGENTS.md` | Instruções locais para Agents: comandos, convenções, escopo e verificações. | É contexto de trabalho, não mecanismo de autorização. |
| `dev-agent.yaml` | Declara composição: Agent → Skills → Tools → limites/configuração. | Pode ser substituído por código tipado. |
| `pyproject.toml` | Dependências, qualidade e comandos do projeto Python. | Em outra stack, use o manifesto equivalente. |
| `.env.example` | Contrato dos nomes de configuração e segredos esperados. | Nunca inclua valores reais nem arquivos `.env` no Git. |
| `.gitignore` | Exclui caches, logs, sessões locais e segredos. | Revise especialmente com Tools de escrita. |

## Como evitar dependências erradas

Um desenho de baixo acoplamento usa portas e contratos:

```text
agents/ ── depende de ──► schemas/ + contratos de tools/
core/   ── depende de ──► schemas/ + interfaces
tools/  ── depende de ──► policies/ + contratos
adapters/providers/ ───► implementam interfaces para fora

Nenhum Agent ──────────► importa SDK de LLM, Docker ou Git diretamente
Nenhum Provider ───────► decide qual Agent atende o usuário
```

Aplicação dos princípios:

- **Separation of Concerns:** orquestração, comportamento, efeitos externos, estado e governança têm locais distintos.
- **Single Responsibility Principle:** uma Tool lê arquivos; ela não decide a estratégia de implementação. Um Provider chama um modelo; ele não roteia tarefas.
- **Alta coesão:** arquivos que mudam juntos ficam juntos, como instruções e checklist de uma Skill.
- **Baixo acoplamento:** Agents falam com contratos; integrações externas ficam em Adapters/Providers substituíveis.
- **Modularidade:** cada Agent, Skill e Tool pode ganhar testes e avaliação sem alterar o runtime inteiro.

## Estruturas que evoluem com a necessidade

### Harness mínimo

Para aprender ou automatizar uma tarefa de desenvolvimento delimitada, mantenha a superfície pequena.

```text
dev-harness/
├── README.md
├── AGENTS.md
├── pyproject.toml
├── app/
│   ├── core.py           # loop simples, contexto e execução
│   ├── agent.py          # um Agent e seu prompt
│   └── tools.py          # 1–3 Tools restritas
├── tests/
└── .env.example
```

**Por que basta:** há um objetivo, um papel e poucas ações. `core`, Agent, prompt e Tools existem conceitualmente, mesmo que coabitem em `app/`. Inclua limites de permissões desde o início; não é preciso criar memória, workflows ou subagents.

### Harness intermediário

Quando surgem múltiplos papéis, conhecimento reutilizado e configuração por ambiente, separe por responsabilidade.

```text
dev-harness/
├── core/
│   ├── orchestrator/
│   ├── executor/
│   └── context/
├── agents/
│   ├── coding/
│   ├── testing/
│   └── review/
├── skills/
│   ├── pytest/
│   └── code-review/
├── prompts/
├── tools/
│   ├── filesystem/
│   ├── git/
│   └── testing/
├── config/
├── schemas/
├── tests/
└── docs/
```

**Motivo da evolução:** os papéis agora mudam separadamente, Tools precisam de contratos e as instruções são reutilizadas. Um Router simples e Policies junto das Tools normalmente ainda são suficientes.

### Harness avançado

Para execução assíncrona, múltiplos provedores, tarefas de alto impacto ou operação contínua, introduza controles explícitos.

```text
dev-harness/
├── core/
├── agents/
├── subagents/
├── skills/
├── prompts/
├── tools/
├── workflows/
├── config/
├── policies/
├── context/
├── memory/
├── sessions/
├── hooks/
├── adapters/
├── providers/
├── schemas/
├── sandbox/
├── evals/
├── tests/
├── logs/                 # ou integração com observabilidade externa
├── cache/
├── templates/
└── docs/
```

**Motivo da evolução:** autonomia e escala criam problemas de estado, segurança, custo, auditoria e regressão. O ganho não vem de ter todas as pastas; vem de tornar explícitas as fronteiras que já passaram a existir.

## Critérios práticos para adicionar uma pasta

Crie um diretório novo quando a resposta a pelo menos uma pergunta for "sim":

1. Esta responsabilidade muda em ritmo diferente das que já existem?
2. Ela tem contrato próprio que precisa de testes ou revisão especializada?
3. Há mais de uma implementação concreta que deve ser intercambiável?
4. Ela contém dados com retenção, segurança ou versionamento diferente?
5. Duas ou mais partes do Harness precisam reutilizá-la sem depender de detalhes internos?

Se todas forem "não", prefira um módulo bem nomeado perto de seu consumidor. Exemplos de superabstração: `workflows/` para uma única sequência fixa, `providers/` para um SDK jamais substituído, `memory/` para salvar histórico bruto, ou `subagents/` para tarefas que uma função determinística resolve.

## Exemplo de composição declarativa

Uma configuração de composição torna visível a relação entre papel, capacidade e limites, sem transformar YAML em lógica de negócio:

```yaml
# dev-agent.yaml
agents:
  coding:
    prompt: prompts/agents/coding.md
    skills: [skills/pytest, skills/migration-safety]
    tools: [filesystem.read, filesystem.patch, testing.run, git.diff]
    token_budget: 18000
    require_approval_for: [filesystem.patch]

workflows:
  feature_delivery:
    steps: [architecture, coding, testing, review]

policies:
  filesystem:
    writable_roots: [workspace]
  shell:
    network: false
```

O arquivo declara **o que compõe** o sistema. A validação desses limites e a execução dos passos continuam pertencendo a `core/`, `policies/` e `tools/`.

## Mapa mental

```text
Agent Harness
│
├── Runtime (fundamental)
│   └── core/
│       ├── Orchestrator
│       ├── Router
│       ├── Executor
│       ├── Lifecycle
│       ├── Context Management
│       └── Token Management
│
├── Inteligência e comportamento
│   ├── agents/          # papéis principais
│   ├── subagents/       # delegação delimitada
│   ├── skills/          # procedimentos reutilizáveis
│   └── prompts/         # instruções versionadas
│
├── Execução e integração
│   ├── tools/           # capacidades controladas
│   ├── workflows/       # ordem explícita dos passos
│   ├── hooks/           # extensões por evento
│   ├── adapters/        # tradução para infraestrutura
│   ├── providers/       # LLMs, embeddings e moderação
│   └── sandbox/         # isolamento técnico
│
├── Controle e contratos
│   ├── config/          # parâmetros por ambiente
│   ├── policies/        # autorização, custo e segurança
│   └── schemas/         # I/O e eventos validados
│
├── Estado
│   ├── context/         # fontes e recuperação de contexto
│   ├── memory/          # conhecimento persistente
│   ├── sessions/        # estado retomável da execução
│   └── cache/           # dados regeneráveis
│
└── Qualidade e operação
    ├── tests/           # corretude determinística e integração
    ├── evals/           # qualidade comportamental
    ├── logs/            # observabilidade/auditoria
    ├── templates/       # formatos reutilizáveis
    └── docs/            # operação e decisões
```

## Checklist de desenho

- [ ] O Harness tem uma fronteira clara entre decisão do Agent e execução de Tool?
- [ ] As Tools validam argumentos e aplicam políticas antes de causar efeitos?
- [ ] O contexto possui fonte, relevância e orçamento, em vez de crescer sem controle?
- [ ] Há testes para roteamento, schemas, policies e integrações críticas?
- [ ] SubAgents e Workflows existem por necessidade mensurável, não por estética?
- [ ] Dados sensíveis, logs, cache e sessões têm retenção e exclusão do Git adequadas?
- [ ] Provedores e Adapters são separados apenas se a troca/isolamento realmente importa?
- [ ] A estrutura atual é a menor que deixa responsabilidades importantes explícitas?

Veja também: [[Agent]], [[SubAgent]], [[Skill]], [[Tool]], [[Prompt Engineering]], [[Workflow]], [[Orchestrator]], [[Context Engineering]], [[Agent Memory]], [[Human in the Loop]].

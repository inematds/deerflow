# Plano mestre do curso — DeerFlow 2.0

> Documento-âncora do curso. Define a arquitetura, os módulos, os labs e os entregáveis.
> Os conteúdos detalhados de cada módulo moram em `track-1-fundamentos/`, `track-2-avancado/` e `comparativo/`.

---

## Visão geral

O DeerFlow 2.0 é um **super agent harness** open-source que orquestra **sub-agents**, **memória** e **sandboxes** por meio de **skills extensíveis**. A versão 2 é uma reescrita completa e não compartilha código com a v1 (Deep Research).

O curso é estruturado em **dois tracks + um módulo transversal**:

- **Track 1 — Fundamentos**: o que é, como instalar, como usar — sem mexer em código.
- **Track 2 — Avançado**: como o harness funciona por dentro e como estendê-lo.
- **Módulo transversal — Comparativo**: DeerFlow vs. Claude Code vs. Antigravity.

Formato sugerido: aulas de ~20–40 min + **lab prático** em cada módulo. O DeerFlow é uma plataforma "mão na massa" — só teoria não sedimenta.

---

## Track 1 — Fundamentos ("O que é e como usar")

Público-alvo: devs curiosos, PMs técnicos, pesquisadores. Não exige Python avançado.

### Módulo 0 — Big picture (30 min, só teoria)

- O que é um **super agent harness** (vs. "chatbot", vs. "framework de agente", vs. "IDE com IA")
- A virada da v1 (Deep Research) para a v2 (harness genérico)
- Os 5 pilares: **Skills, Sub-agents, Sandbox, Context Engineering, Memory**
- Casos de uso reais: deep research, geração de relatórios, code review, newsletters, PPTs, podcasts, análise de dados
- **Sem lab.**

### Módulo 1 — Instalação e primeiro "olá mundo"

- `make setup` (wizard interativo) vs. `make config` (manual)
- `config.yaml` + `.env`: o que mora onde e por quê
- Escolher um provider (OpenAI, OpenRouter, Claude Code OAuth, Codex CLI, vLLM local, Doubao/DeepSeek/Kimi)
- `make doctor` — o amigo que ninguém usa
- **Lab**: subir via Docker (`make docker-init && make docker-start`), abrir o frontend, rodar a primeira pergunta.

### Módulo 2 — A interface e o loop básico

- Tour pelo frontend Next.js
- Anatomia de uma conversa: prompt → plano → execução → resultado
- **Plan mode**
- **Thinking mode** e quando ligar
- Streaming de respostas
- **Lab**: pedir uma pesquisa simples, ativar plan mode, comparar com/sem thinking.

### Módulo 3 — Skills: a capacidade principal

- O que é uma skill, por que existe
- Tour pelo catálogo público de skills do DeerFlow:
  - `deep-research`, `github-deep-research`, `systematic-literature-review`
  - `data-analysis`, `chart-visualization`, `academic-paper-review`
  - `ppt-generation`, `podcast-generation`, `newsletter-generation`, `video-generation`, `image-generation`
  - `frontend-design`, `vercel-deploy-claimable`, `code-documentation`
  - `consulting-analysis`, `surprise-me`
- Como invocar uma skill e como o agente decide sozinho qual usar
- **Lab**: rodar `deep-research` em um tema e comparar com `systematic-literature-review`.

### Módulo 4 — Web search, arquivos e sandbox (nível usuário)

- Providers de web search (Tavily, InfoQuest)
- Upload de arquivos
- Sandbox: **Local / Docker / Kubernetes** — o que muda e quando escolher cada um
- **Lab**: subir um CSV, pedir análise com geração de gráficos (usa `data-analysis` + `chart-visualization`).

### Módulo 5 — Memória de longo prazo

- O que o DeerFlow lembra entre sessões
- Como revisar, editar e limpar memórias
- Boas práticas: quando memória ajuda, quando atrapalha
- **Lab**: criar um projeto de pesquisa contínuo em 3 sessões, verificando o que é carregado automaticamente.

### Módulo 6 — Integrações de bolso

- Canais IM: Telegram, Slack, Feishu, WeChat, WeCom
- Tracing: LangSmith ou Langfuse (para ver o que o agente está fazendo)
- **Lab**: plugar um bot de Telegram e conversar com o DeerFlow pelo celular.

**Entregável do Track 1**: o aluno consegue rodar, configurar e usar o DeerFlow para tarefas reais sem precisar tocar código.

---

## Track 2 — Avançado ("Para quem vai estender o harness")

Público-alvo: engenheiros de IA, platform engineers, contribuidores upstream.
Pré-requisito: Track 1 + Python 3.12 confortável + noção de LangGraph/LangChain.

### Módulo A1 — Arquitetura profunda

- Leitura guiada da documentação de arquitetura do DeerFlow (`ARCHITECTURE.md`, `HARNESS_APP_SPLIT.md`)
- **LangGraph no DeerFlow**: o grafo do `lead_agent`, nodes, edges, state
- O split harness ↔ app (o que é "harness" e o que é "aplicação" rodando dentro)
- **Context engineering** na prática: janela, compactação, summarization
- **Guardrails**
- **Lab**: ativar LangSmith, rodar uma query complexa, traçar o grafo de execução inteiro e identificar onde o tempo foi gasto.

### Módulo A2 — Middleware e execução

- Leitura guiada do fluxo de execução do middleware
- Como o `lead_agent` despacha para custom agents (via `agent_name`)
- Ciclo de vida de uma tool call e recuperação de erros
- **Lab**: escrever um middleware que loga toda ferramenta chamada e seu tempo.

### Módulo A3 — Criando a sua primeira skill

- Anatomia de uma skill no catálogo público
- A meta-skill `skill-creator`: como o DeerFlow cria skills
- Descrições de skill: a arte de fazer o agente **acionar na hora certa** (trigger accuracy)
- Testes e validação
- **Lab**: construir do zero uma skill `brazilian-gov-research` que cruza dados do IBGE + Portal da Transparência.

### Módulo A4 — Sub-agents e custom agents

- A diferença entre **sub-agent** (chamado dentro de uma task) e **custom agent** (roteado pelo `lead_agent`)
- SOUL/config de um custom agent
- RFC `rfc-create-deerflow-agent.md` — leitura guiada
- Quando criar um custom agent vs. uma skill nova (decisão de design)
- **Lab**: criar um custom agent `security-auditor` com SOUL próprio e plugar num canal Slack dedicado.

### Módulo A5 — Tools customizadas, MCP e grep/glob

- Leitura guiada das RFCs de tools (`rfc-grep-glob-tools.md`) e do guia de MCP server
- Expor um serviço interno como **MCP server** e conectá-lo ao DeerFlow
- OAuth flows (`client_credentials`, `refresh_token`) para MCP HTTP/SSE
- **Lab**: expor uma API interna como MCP e fazer o agente consultá-la numa skill existente.

### Módulo A6 — Providers de modelo e roteamento

- `langchain_openai:ChatOpenAI` com `base_url` (OpenRouter, LiteLLM, gateways)
- `use_responses_api: true` (OpenAI `/v1/responses`)
- **vLLM local** com `VllmChatModel` e reasoning models (Qwen3, DeepSeek)
- **CLI-backed providers**: Codex CLI, Claude Code OAuth
- Quando usar cada um: custo vs. latência vs. qualidade
- **Lab**: rodar a mesma tarefa em 3 modelos diferentes e comparar traces/custo.

### Módulo A7 — Sandbox, file system e segurança

- Modos **Local / Docker / Kubernetes** — implicações reais de segurança
- Isolamento, quotas, imagem do sandbox
- Leitura guiada de `SECURITY.md` e dos alertas de deploy do README
- **Lab**: montar um deploy K8s com provisioner e testar um script malicioso (falha esperada).

### Módulo A8 — Memória avançada e context engineering

- Leitura guiada dos documentos de memória (`MEMORY_IMPROVEMENTS.md` e correlatos)
- Estratégias de compactação, seleção, recall
- Como evitar "memory poisoning" e entradas obsoletas
- **Lab**: escrever um teste que prova que uma memória antiga é corretamente sobrescrita.

### Módulo A9 — Embutindo o DeerFlow em outro app

- **Embedded Python Client**
- API gateway e documentação de API
- Auto title generation, streaming server-side
- **Lab**: embutir o DeerFlow como biblioteca num script Python e rodar uma skill programaticamente.

### Módulo A10 — Contribuindo

- Fluxo de PR (`CONTRIBUTING.md`, `AGENTS.md` de backend e frontend)
- Leitura guiada dos planos em `docs/plans/` e dos RFCs (`rfc-*`)
- Como achar um "good first issue" — a git log recente mostra padrão: fixes de middleware, guardrails, async wrapping, IM channels
- **Lab**: pegar um item do `TODO.md` do backend e abrir um draft PR.

**Entregável do Track 2**: o aluno consegue estender o harness (skill nova, custom agent, MCP server, provider), embutir em outro sistema e contribuir upstream.

---

## Módulo transversal — Comparativo

> Pode rodar depois do Módulo 3 do Track 1 ou como aula standalone.
> A ideia é dar clareza de **quando usar o quê**, sem cair em "X é melhor que Y".

### Framework de comparação (matriz fixa)

Usar estes eixos para qualquer plataforma:

1. **Categoria** (IDE-agent, harness, framework, SaaS)
2. **Onde roda** (local, cloud, hybrid)
3. **Modelo de extensibilidade** (skills, tools, extensions, plugins)
4. **Execução de código** (sandbox, tipo de isolamento)
5. **Memória** (nenhuma, sessão, long-term)
6. **Sub-agentes / orquestração**
7. **Modelos suportados** (single vendor vs. multi)
8. **Interface** (CLI, IDE, chat web, IM)
9. **Open source?**
10. **Público-alvo primário**

### Posicionamento (esqueleto honesto — a validar com os alunos)

- **Claude Code** — CLI/IDE-agent focado em **engenharia de software**, Anthropic only, skills + MCP, sandbox via host, excelente em code tasks. É o "par de programação".
- **Antigravity (Google)** — IDE-agent recente focado em agentes autônomos dentro do editor, multi-agente, integrado ao ecossistema Google/Gemini. É a "IDE pensada em agentes".
- **DeerFlow 2.0** — **harness genérico e open-source**, multi-model, multi-channel (inclui IM), com skills para domínios muito além de código (research, podcast, PPT, video), sandbox configurável até K8s, memória de longo prazo de primeira classe. É o "harness que você hospeda e estende".

> **Nota didática**: como Antigravity é recente, o curso deve **demonstrar ao vivo** as 3 plataformas resolvendo a **mesma tarefa** em vez de listar features — é mais honesto e mais didático.

### Tarefas de comparação sugeridas (labs comparativos)

1. **Tarefa de código**: refatorar um módulo Python e abrir PR → força de Claude Code/Antigravity.
2. **Tarefa de research**: "me faça um relatório sobre X com 10 fontes citadas" → força do DeerFlow.
3. **Tarefa híbrida**: "analise esse CSV, gere um PPT e poste num Slack" → DeerFlow ganha naturalmente.
4. **Tarefa de extensibilidade**: criar uma skill/tool nova em cada um → comparar DX e esforço.

**Entregável do módulo**: o aluno sabe escolher a ferramenta certa por tipo de tarefa, não por moda.

---

## Decisões em aberto (checklist antes de produzir conteúdo)

- [ ] **Formato final** — vídeo, texto, HTML interativo, Notion, LMS?
- [ ] **Idioma** — PT-BR, inglês, ou bilíngue?
- [ ] **Profundidade prática dos labs** — roteiro exato (comandos, prompts, arquivos esperados) ou só descrição de alto nível?
- [ ] **Público do Track 2** — engenheiros de backend Python "genéricos" ou gente que já mexeu com LangGraph/agentes? (muda o tom do módulo A1)
- [ ] **Duração-alvo** — curso completo de ~20h, ou 2 mini-cursos de 6–8h cada?
- [ ] **Comparativo** — fazer pesquisa a fundo sobre Antigravity antes de gravar, ou assumir demonstração ao vivo?

Respondidas essas decisões, o plano é expandir **módulo a módulo** com roteiro de aula, slides-chave e código/prompts dos labs.

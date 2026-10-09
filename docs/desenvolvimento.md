# Como vamos desenvolver

Desenvolvimento **solo, com apoio de IA**. O processo existe para que cada sessão com a IA comece com o contexto certo e para que o código continue coerente ao longo de meses.

## Roadmap

| Fase | Entrega | Pronto quando | Foco de estudo |
|---|---|---|---|
| **0. Fundação** | Monorepo, Docker Compose, CI, `openapi.yaml` inicial, auth (Google/Discord/link mágico), deploy no VPS com HTTPS | Consigo fazer login em produção | Go, OAuth |
| **1. MVP** | Mesa/Campanha/convites, Templates + Sistema D&D 5e (SRD 5.2) + Fichas com fórmulas (WASM), Estado em tempo real (SSE), Notas com Visibilidade, Sessões + Presença, Assistente com RAG, Resumo, medição de tokens | Meu grupo joga uma sessão real usando só o app | **RAG**, WASM, Mongo |
| **2. MCP** | Servidor MCP (Tools, Resources, Prompts), OAuth 2.1, rastreamento, "Apps conectados", escrita via MCP | Consulto minha campanha pelo Claude e vejo o uso no painel | **MCP** |
| **3. Mestre** | Ingestão de PDF e foto de caderno → Notas em rascunho, IA criadora de história (Opus 5.5), escrita pelo Assistente com confirmação | Um caderno digitalizado vira Notas pesquisáveis | Extração estruturada |
| **4. Mapas** | Editor de mapas e exportação para impressão em escala | Imprimo um mapa e jogo com miniaturas | Canvas |
| **5. Mapa jogável** | Mapa sincronizado ao vivo (WebSocket) | Uma sessão remota usa o mapa | Tempo real |
| **6. Comercial** | Planos, cotas, cobrança (Stripe) | Primeiro pagamento | — |
| **Futuro** | Videochamada (LiveKit), Assistente como cliente MCP, MCP Apps | — | — |

A Fase 6 pode subir para logo depois da 3 se for desejável cobrar mais cedo, já que a medição de tokens existe desde a Fase 1.

### Fase 0 em detalhe
1. Estrutura do monorepo (ver [`arquitetura.md`](arquitetura.md#estrutura-do-repositório)).
2. `deploy/docker-compose.yml` com Postgres (imagem com pgvector) e Mongo. `server` com `migrate`, `api` e `worker`.
3. CI no GitHub Actions: lint (`golangci-lint`, `eslint`), testes Go com testcontainers, testes do frontend e build das imagens.
4. `api/openapi.yaml` com o primeiro endpoint (`GET /me`) e geração de código no Go e no TS.
5. Auth (UC-01, UC-02, UC-03).
6. VPS: Docker Compose de produção, Caddy, backup diário no R2 e deploy feito pela CI ao fazer merge na `main`.

## Fluxo de trabalho

1. **Backlog no GitHub Issues**: uma issue por caso de uso (`UC-xx`, ver [`casos-de-uso.md`](casos-de-uso.md)), com critérios de aceite e as regras de negócio (`RN-xx`) envolvidas. Um GitHub Project organiza as issues por fase.
2. **Uma branch por issue** (`uc-50-assistente-consulta`). Commits no padrão *Conventional Commits* (`feat:`, `fix:`, `docs:`…), em inglês.
3. **Pull Request** referenciando a issue (`Closes #12`). A CI precisa passar antes do merge, mesmo trabalhando sozinho. O PR também funciona como registro do que foi feito e por quê.
4. **Merge na `main` → deploy automático.**

## Trabalhando com a IA

- **`CLAUDE.md`** na raiz: as convenções que toda sessão precisa seguir.
- **`CONTEXT.md`**: o glossário. Termo novo ou ambíguo → atualizar o glossário **antes** do código.
- **ADRs em `docs/adr/`**: decisões difíceis de reverter, surpreendentes e resultado de um trade-off real. Antes de "corrigir" algo que parece estranho, verificar se há um ADR.
- **Planejar antes de implementar**: para cada issue, pedir à IA um plano (arquivos, testes, migrações) e revisar esse plano antes de mandar escrever código.
- **RAG e MCP**: todo PR que toca esses módulos explica o mecanismo na descrição e atualiza [`rag.md`](rag.md) / [`mcp.md`](mcp.md). Eles são material de estudo.

### Skills do Claude Code usadas

Registro das skills usadas no projeto: quando foram usadas, o que produziram e quando vale usar de novo.

| Skill | Quando | O que produziu | Usar de novo quando |
|---|---|---|---|
| `mattpocock-skills:grill-with-docs` | Planejamento inicial (2026-10) | Ponto de entrada que carrega as duas skills abaixo | For preciso desenhar uma funcionalidade nova ou revisar uma fase antes de implementar |
| `mattpocock-skills:grilling` | Carregada pela `grill-with-docs` | Entrevista em rodadas numeradas (Q1–Q37), cada pergunta com uma recomendação, cobrindo todas as decisões em aberto | Houver muitas decisões interdependentes ainda abertas |
| `mattpocock-skills:domain-modeling` | Carregada pela `grill-with-docs` | O glossário em `CONTEXT.md` e os ADRs 0001–0005 em `docs/adr/` | Um termo do domínio surgir ou mudar, ou uma decisão atender aos critérios de ADR |
| `claude-api` | Carregada no início do planejamento e interrompida, sem uso | — | Implementar o Assistente (UC-50), o Resumo e a Fase 3. Ela traz o uso correto do `anthropic-sdk-go`, do Tool Runner e do *prompt caching* |

## Testes

| O quê | Como | Quando |
|---|---|---|
| Domínio: `access` (Visibilidade), `formula`, regras de `sheet` | **TDD**: teste primeiro, tabela de casos | Sempre |
| Persistência e consultas | Testes de integração com **testcontainers** (Postgres e Mongo reais) | Toda consulta nova |
| API | Testes HTTP contra o servidor real em memória | Todo endpoint |
| Frontend | Vitest + Testing Library | Componentes com lógica |
| Fluxo principal | Playwright: login → criar Campanha → Ficha → Nota → perguntar ao Assistente | Na CI |
| RAG | `evals/rag/`: recuperação e vazamento em todo PR que toca `retrieval`/`access`/`toolreg`, e avaliação de resposta manual ([`rag.md`](rag.md#7-avaliação)) | Ver ao lado |
| MCP | Testes com o cliente do go-sdk + MCP Inspector ([`mcp.md`](mcp.md#8-testes)) | Fase 2 |

**Os testes de vazamento são bloqueantes.** Um teste de Visibilidade falhando impede o merge, sem exceção.

## Definição de pronto

Uma issue está pronta quando:
- os critérios de aceite estão cobertos por testes;
- a CI está verde;
- o `openapi.yaml` está atualizado (se a API mudou);
- o `CONTEXT.md` está atualizado (se um termo surgiu ou mudou);
- há um ADR, se a decisão se encaixa nos critérios;
- os textos da interface estão no arquivo de i18n, sem texto fixo no código;
- a funcionalidade foi verificada rodando o app, não só pelos testes.

## Ambientes

| Ambiente | Onde | Dados |
|---|---|---|
| Local | `docker compose` + `vite dev` | Seed com a campanha de exemplo das avaliações |
| Produção | VPS | Reais, com backup diário no R2 |

Um ambiente de homologação só será criado se for necessário (por exemplo, para testar o MCP com o Claude via HTTPS). Até lá, um túnel (Cloudflare Tunnel) até a máquina local resolve.

## Convenções

- **Código, commits, nomes de banco, infraestrutura e logs em inglês.** Interface e documentação de produto em pt-BR.
- Banco: tabelas e colunas em **snake_case, singular** (`character_sheet`, `sheet_state`). Exceções por palavra reservada: `play_group`, `app_user`.
- Termos do domínio no código seguem a coluna _Código_ do `CONTEXT.md` (`Group`, `Campaign`, `CharacterSheet`…).
- IDs: UUID v7 (ordenáveis por tempo).
- Datas sempre em UTC no banco (`timestamptz`).

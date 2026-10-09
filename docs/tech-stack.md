# Tech stack

Critério geral: **custo baixo de infraestrutura hoje, sem impedir que o produto vire comercial amanhã**. Tudo em containers, para que trocar de hospedagem seja só trocar de máquina.

## Backend

| Item | Escolha | Por quê |
|---|---|---|
| Linguagem | **Go** (última versão estável) | Definido no project.MD. Um binário pequeno, ótimo para um VPS barato |
| HTTP | `net/http` + `chi` | Leve e idiomático, com middlewares simples |
| Contrato | **OpenAPI escrito primeiro** (`api/openapi.yaml`), servidor gerado com `oapi-codegen` | Uma fonte da verdade para Go e TS |
| Banco principal | **PostgreSQL** + extensão **pgvector** | ADR 0001 e ADR 0004 |
| Acesso ao Postgres | `pgx` + **`sqlc`** (SQL escrito à mão, código gerado) | Sem ORM. A regra de Visibilidade fica visível no SQL |
| Migrações | `goose` | Arquivos SQL versionados |
| Banco de documentos | **MongoDB** + driver oficial Go | Conversas e mapas (ADR 0001) |
| Fila de jobs | **River** | Fila dentro do Postgres, sem Redis |
| Fórmulas | `expr-lang/expr`, compilado para WASM (`GOOS=js GOARCH=wasm`) | ADR 0003. Medir o tamanho do `.wasm`. TinyGo é uma otimização possível, se for compatível |
| IA | **`anthropic-sdk-go`** (SDK oficial) com o Tool Runner | Assistente e Resumo |
| Embeddings | **Voyage AI** (modelo da família atual, ex.: `voyage-3.5-lite`, 1024 dimensões) | A Anthropic não oferece embeddings, e a Voyage é a parceira recomendada |
| MCP | `modelcontextprotocol/go-sdk` (SDK oficial) | Fase 2, ADR 0005 |
| Auth | Implementação própria: OAuth Google/Discord + link mágico | ADR 0002 |
| Logs | `log/slog` em JSON | Biblioteca padrão |
| Erros | Sentry (plano gratuito) | Alertas sem custo |

### Modelos do Claude

| Uso | Modelo | Observação |
|---|---|---|
| Assistente (chat com ferramentas) | `claude-sonnet-5-5` | Bom equilíbrio entre qualidade e custo para tool use |
| Resumo da Sessão | `claude-haiku-5-5` | Barato, bom para resumir |
| IA criadora de histórias (Fase 3) | `claude-opus-5-5` | Melhor narrativa e raciocínio |
| Extração de PDF e caderno (Fase 3) | A definir na Fase 3 | O Claude lê PDF e imagem direto |

Os modelos são configurados por variável de ambiente. Toda chamada usa *prompt caching* no system prompt e nas ferramentas, e grava o uso em `ai_usage`.

## Frontend

| Item | Escolha |
|---|---|
| Base | **React** + TypeScript + Vite (SPA, sem SSR) |
| Rotas e dados | TanStack Router + TanStack Query |
| UI | Tailwind CSS + shadcn/ui |
| Cliente da API | Gerado com `openapi-typescript` |
| Editor de Notas | A definir (candidatos: TipTap, Lexical) |
| Tempo real | `EventSource` (SSE) |
| PWA | `vite-plugin-pwa` |
| i18n | `react-i18next`, interface em pt-BR |
| Mapas (Fase 4) | A definir (candidatos: Konva, PixiJS) |

## Infraestrutura

| Item | Escolha | Custo estimado |
|---|---|---|
| Código e CI | GitHub + GitHub Actions | Grátis |
| Imagens | GitHub Container Registry | Grátis |
| Servidor | VPS Hetzner com Docker Compose | ~€5–8/mês |
| HTTPS / proxy | Caddy (certificados automáticos) | — |
| Arquivos e backups | Cloudflare R2 | Grátis até 10 GB, sem custo de saída |
| E-mail (link mágico) | **A definir** (candidatos: Resend, Amazon SES) | Plano gratuito |
| Erros | Sentry | Grátis |

## Testes

| Camada | Ferramenta |
|---|---|
| Go, unidade e integração | `testing` + **testcontainers** (Postgres e Mongo reais) |
| Frontend | Vitest + Testing Library |
| Ponta a ponta | Playwright (fluxo principal do MVP) |
| Assistente / RAG | Conjunto de avaliação em `evals/rag/` (ver [`rag.md`](rag.md#7-avaliação)) |
| MCP | MCP Inspector + testes com o cliente do `go-sdk` (ver [`mcp.md`](mcp.md#8-testes)) |

# RPG Note

Plataforma para jogadores e mestres de RPG de mesa centralizarem suas anotações, com um Assistente de IA que consulta essas anotações. Desenvolvimento solo com apoio de IA. O projeto também serve para estudar **RAG**, **MCP** e **MongoDB**.

## Leia antes de trabalhar
- `CONTEXT.md`: glossário do domínio. Use exatamente esses termos, e no código o nome da coluna _Código_.
- `docs/casos-de-uso.md`: casos de uso (`UC-xx`) e regras de negócio (`RN-xx`).
- `docs/arquitetura.md`, `docs/tech-stack.md`, `docs/desenvolvimento.md`.
- `docs/rag.md` e `docs/mcp.md` antes de tocar em `retrieval`, `toolreg`, `assistant` ou `mcp`.
- `docs/adr/`: antes de mudar algo que parece estranho, verifique se há um ADR que o explique.

## Regras que não podem ser quebradas
- **Visibilidade**: o acesso a Notas e Fichas passa pelo módulo `access` e é filtrado **em SQL**. Nunca filtrar no prompt nem no frontend. Isso vale também para o Assistente e o MCP (RN-07).
- `user_id` e `campaign_id` das ferramentas do Assistente/MCP vêm do contexto da requisição, nunca da entrada do modelo.
- Mongo só guarda documentos de dono único, sem Visibilidade cruzada (ADR 0001). Na dúvida, o dado vai para o Postgres.
- Toda chamada de IA registra tokens em `ai_usage`, e toda chamada MCP registra uma linha em `mcp_call`.

## Convenções
- Código, commits (Conventional Commits), banco, infraestrutura e logs em **inglês**. Interface em **pt-BR** via i18n, sem texto fixo no código.
- Banco: snake_case no **singular** (`character_sheet`, `sheet_state`). Exceções por palavra reservada: `play_group`, `app_user`.
- Postgres via `sqlc` (SQL escrito à mão, sem ORM). Migrações com `goose`.
- API definida primeiro em `api/openapi.yaml`, e o código é gerado a partir dela.
- TDD em `access`, `formula` e nas regras de `sheet`. Testes de integração com testcontainers, não mocks de banco.
- Um módulo em `server/internal/` não acessa as tabelas de outro módulo.

## Como trabalhar
- Cada tarefa corresponde a uma issue `UC-xx`. Proponha um plano (arquivos, migrações, testes) antes de implementar.
- Termo novo ou ambíguo: atualizar o `CONTEXT.md` antes do código.
- **RAG e MCP**: explique sempre o mecanismo em detalhe (fluxo, onde vive no código, trade-offs, como medir) e atualize `docs/rag.md` / `docs/mcp.md`. O autor está estudando esses temas.
- Skills do Claude Code já usadas e quando reutilizá-las: `docs/desenvolvimento.md` → "Skills do Claude Code usadas". Ao usar uma skill nova, registre-a lá.

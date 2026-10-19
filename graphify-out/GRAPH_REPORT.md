# Graph Report - rpg-note  (2026-10-15)

## Corpus Check
- Corpus is ~10,886 words - fits in a single context window. You may not need a graph.

## Summary
- 127 nodes · 199 edges · 12 communities (9 shown, 3 thin omitted)
- Extraction: 88% EXTRACTED · 11% INFERRED · 1% AMBIGUOUS · INFERRED: 22 edges (avg confidence: 0.82)
- Token cost: 79,320 input · 0 output

## Community Hubs (Navigation)
- Docs e Decisões (ADRs)
- Pipeline RAG e Indexação
- Roadmap, MCP SDK e Avaliação
- Assistente e Conversa
- Fichas e Tempo Real
- Notas, Visibilidade e Tools
- Uso de IA e Tracking MCP
- Mesa, Campanha e Sessão
- OAuth MCP e Filtro SQL
- Tabela character_sheet
- TODO do Autor

## God Nodes (most connected - your core abstractions)
1. `CLAUDE.md (AI dev instructions)` - 17 edges
2. `Module toolreg (shared tool registry)` - 14 edges
3. `Module access (single Visibility rule)` - 10 edges
4. `Tech stack` - 8 edges
5. `Module mcp` - 8 edges
6. `MCP no RPG Note` - 7 edges
7. `ADR 0001: Postgres principal, Mongo para documentos isolados` - 7 edges
8. `ADR 0005: Servidor MCP remoto com OAuth proprio` - 7 edges
9. `Campanha (Campaign)` - 7 edges
10. `Module assistant` - 7 edges

## Surprising Connections (you probably didn't know these)
- `CLAUDE.md (AI dev instructions)` --references--> `TDD on access, formula, sheet rules`  [EXTRACTED]
  CLAUDE.md → docs/desenvolvimento.md
- `CLAUDE.md (AI dev instructions)` --references--> `pgx + sqlc (no ORM)`  [EXTRACTED]
  CLAUDE.md → docs/tech-stack.md
- `RN-09 Personal Session Recap` --references--> `Resumo (SessionRecap)`  [EXTRACTED]
  docs/casos-de-uso.md → CONTEXT.md
- `CLAUDE.md (AI dev instructions)` --references--> `ADR 0001: Postgres principal, Mongo para documentos isolados`  [EXTRACTED]
  CLAUDE.md → docs/adr/0001-postgres-principal-mongo-para-documentos-isolados.md
- `CLAUDE.md (AI dev instructions)` --references--> `Module access (single Visibility rule)`  [EXTRACTED]
  CLAUDE.md → docs/arquitetura.md

## Import Cycles
- None detected.

## Hyperedges (group relationships)
- **Visibility enforcement chain (access predicate shared by UI, Assistant, MCP)** — docs_arquitetura_readable_note, docs_arquitetura_access, docs_arquitetura_note, docs_arquitetura_retrieval, docs_arquitetura_toolreg, docs_arquitetura_mcp, docs_casos_de_uso_rn_07 [EXTRACTED 1.00]
- **RAG indexing pipeline** — docs_casos_de_uso_uc_30, docs_arquitetura_river, docs_rag_indexnotejob, docs_rag_chunking, docs_rag_embedding, docs_tech_stack_voyage_ai, docs_arquitetura_note_chunk [EXTRACTED 1.00]
- **AI and MCP usage tracking** — docs_arquitetura_usage, docs_arquitetura_ai_usage, docs_arquitetura_mcp_call, docs_mcp_tracking_middleware, docs_arquitetura_retrieval_log, docs_casos_de_uso_rn_11 [INFERRED 0.85]

## Communities (12 total, 3 thin omitted)

### Community 0 - "Docs e Decisões (ADRs)"
Cohesion: 0.19
Nodes (19): CLAUDE.md (AI dev instructions), CONTEXT.md (domain glossary), ADR 0002: Autenticacao propria sem senha, ADR 0003: Formulas da Ficha em Go compilado para WASM, ADR 0004: RAG agentico com busca hibrida, ADR 0005: Servidor MCP remoto com OAuth proprio, Arquitetura, Module identity (+11 more)

### Community 1 - "Pipeline RAG e Indexação"
Cohesion: 0.16
Nodes (14): ADR 0001: Postgres principal, Mongo para documentos isolados, Mongo conversation collection, note_chunk table, Module platform, Module retrieval, retrieval_log table, River job queue (in Postgres), UC-30/31 Create/edit Note and Visibility (+6 more)

### Community 2 - "Roadmap, MCP SDK e Avaliação"
Cohesion: 0.12
Nodes (15): Mongo battle_map collection, Module mcp, Roadmap Phases 0-6, modelcontextprotocol/go-sdk, MCP Inspector, Model Context Protocol (JSON-RPC 2.0), MCP primitives: Tools, Resources, Prompts, evals/rag evaluation set (retrieval, answer, leakage) (+7 more)

### Community 3 - "Assistente e Conversa"
Cohesion: 0.18
Nodes (12): Assistente (Assistant), Conversa (Conversation), Module assistant, Cliente MCP (actor), RN-07 Assistant/MCP filter applied in DB, never prompt, RN-08 Conversation readable only by initiator, UC-50/51 Query Notes via Assistant, Citations [[nota:id|Title]] (+4 more)

### Community 4 - "Fichas e Tempo Real"
Cohesion: 0.15
Nodes (14): Estado (SheetState), Ficha (CharacterSheet), Mestre (GameMaster), Sistema (GameSystem), Template (SheetTemplate), Module formula, Module realtime (SSE hub), Module sheet (+6 more)

### Community 5 - "Notas, Visibilidade e Tools"
Cohesion: 0.20
Nodes (14): Nota (Note), Resumo (SessionRecap), Visibilidade (Visibility: private/game_masters/everyone), Module access (single Visibility rule), Module note, Module toolreg (shared tool registry), RN-01 Nota Visibility, RN-02 No one reads others' private Notes (+6 more)

### Community 6 - "Uso de IA e Tracking MCP"
Cohesion: 0.17
Nodes (12): ai_usage table, mcp_call table, session_recap table, Module usage, RN-09 Personal Session Recap, RN-11 Every AI call records tokens, UC-42 Generate Session Recap, UC-61 Query via MCP Client (+4 more)

### Community 7 - "Mesa, Campanha e Sessão"
Cohesion: 0.24
Nodes (10): Campanha (Campaign), Dono (Owner), Jogador (Player), Membro (Member), Mesa (Group), Presenca (Attendance), Sessao (GameSession), Module campaign (+2 more)

### Community 8 - "OAuth MCP e Filtro SQL"
Cohesion: 0.33
Nodes (5): readable_note(user_id) SQL predicate, UC-60 Connect MCP Client, Consent: scopes notes:read/notes:write x authorized Campanhas, OAuth 2.1 authorization server (PKCE S256, DCR, CIMD), pgx + sqlc (no ORM)

## Ambiguous Edges - Review These
- `README (rpg-alive-table)` → `project.MD (original product vision)`  [AMBIGUOUS]
  README.md · relation: conceptually_related_to

## Knowledge Gaps
- **30 isolated node(s):** `README (rpg-alive-table)`, `TODO 15/10 (backend start)`, `Presenca (Attendance)`, `Dono (Owner)`, `Jogador (Player)` (+25 more)
  These have ≤1 connection - possible missing edges or undocumented components. (Counts symbols only; 34 node(s) total have ≤1 connection when file, concept and rationale nodes are included.)
- **3 thin communities (<3 nodes) omitted from report** — run `graphify query` to explore isolated nodes.

## Suggested Questions
_Questions this graph is uniquely positioned to answer:_

- **Why does `Module access (single Visibility rule)` connect `Notas, Visibilidade e Tools` to `Docs e Decisões (ADRs)`, `Pipeline RAG e Indexação`, `Roadmap, MCP SDK e Avaliação`, `Assistente e Conversa`, `Fichas e Tempo Real`, `OAuth MCP e Filtro SQL`?**
  _High betweenness centrality (0.239) - this node is a cross-community bridge._
- **What connects `README (rpg-alive-table)`, `TODO 15/10 (backend start)`, `Presenca (Attendance)` to the rest of the system?**
  _30 weakly-connected nodes found - possible documentation gaps or missing edges._
- **Should `Roadmap, MCP SDK e Avaliação` be split into smaller, more focused modules?**
  _Cohesion score 0.125 - nodes in this community are weakly interconnected._
- **Why does `Module toolreg (shared tool registry)` connect `Notas, Visibilidade e Tools` to `Docs e Decisões (ADRs)`, `Roadmap, MCP SDK e Avaliação`, `Assistente e Conversa`, `Fichas e Tempo Real`, `Uso de IA e Tracking MCP`?**
  _High betweenness centrality (0.236) - this node is a cross-community bridge._
- **Why does `CLAUDE.md (AI dev instructions)` connect `Docs e Decisões (ADRs)` to `Pipeline RAG e Indexação`, `Roadmap, MCP SDK e Avaliação`, `Assistente e Conversa`, `Notas, Visibilidade e Tools`, `Uso de IA e Tracking MCP`, `OAuth MCP e Filtro SQL`?**
  _High betweenness centrality (0.230) - this node is a cross-community bridge._

### Low-confidence Hints
_AMBIGUOUS edges — the extractor was unsure. Verify before acting on these._

- **What is the exact relationship between `README (rpg-alive-table)` and `project.MD (original product vision)`?**
  _Edge tagged AMBIGUOUS (relation: conceptually_related_to) - confidence is low._
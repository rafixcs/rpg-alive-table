# Casos de uso

Os termos seguem o glossário em [`CONTEXT.md`](../CONTEXT.md). Cada caso de uso vira **uma issue no GitHub** com o mesmo ID (ex.: `UC-50`), e cada PR referencia a sua.

## Atores

| Ator | Quem é |
|---|---|
| **Visitante** | Pessoa sem login |
| **Usuário** | Pessoa com conta, em qualquer contexto |
| **Dono** | Usuário que administra uma Mesa |
| **Membro** | Usuário que pertence a uma Mesa |
| **Mestre** | Membro com papel de Mestre numa Campanha |
| **Jogador** | Membro com papel de Jogador numa Campanha |
| **Assistente** | O agente de IA, que sempre age em nome de um Usuário e com as permissões dele |
| **Cliente MCP** | App de IA externo (Claude, ChatGPT, Cursor…) autorizado por um Usuário |
| **Worker** | Processos em segundo plano (indexação, Resumo, extração) |

## Regras de negócio

Valem em todos os casos de uso, incluindo o Assistente e o MCP.

| ID | Regra |
|---|---|
| **RN-01** | Toda Nota tem uma Visibilidade, definida pelo autor: `privada` (só o autor), `mestres` (autor + Mestres) ou `todos` (todos da Campanha, ou todos os Membros quando a Nota é da Mesa). O padrão é `todos`. |
| **RN-02** | Ninguém lê a Nota privada de outra pessoa, nem o Mestre nem o Dono. |
| **RN-03** | Só o autor altera a Visibilidade de uma Nota. O Dono pode **apagar** Notas da Mesa (moderação), mas não pode lê-las se forem privadas. |
| **RN-04** | O Mestre vê todas as Fichas da Campanha, mas só edita o **Estado** das Fichas dos Jogadores. |
| **RN-05** | Um Sistema não pode ser editado. Para alterá-lo, o usuário cria uma cópia, que vira um Template dele. |
| **RN-06** | Uma Ficha pertence a uma única Campanha. Levar o personagem para outra Campanha cria uma cópia ligada à original. |
| **RN-07** | O Assistente e os Clientes MCP só acessam o que o Usuário em nome de quem agem poderia ler. O filtro é aplicado no banco, nunca no prompt. |
| **RN-08** | Uma Conversa só pode ser lida por quem a iniciou. Se uma Nota citada for restringida depois, a Conversa antiga continua visível (ver ADR 0001). |
| **RN-09** | Cada pessoa tem o próprio Resumo de uma Sessão, feito só com as Notas que ela pode ler. |
| **RN-10** | Os papéis de Mestre e Jogador são definidos por Campanha. Uma Campanha pode ter vários Mestres. |
| **RN-11** | Toda chamada de IA registra os tokens consumidos por usuário, Campanha e funcionalidade. |

## Visão geral por fase

| ID | Caso de uso | Ator principal | Fase |
|---|---|---|---|
| UC-01 | Entrar com Google ou Discord | Visitante | 0 |
| UC-02 | Entrar com link mágico por e-mail | Visitante | 0 |
| UC-03 | Sair da conta | Usuário | 0 |
| UC-10 | Criar Mesa (ou campanha avulsa) | Usuário | 1 |
| UC-11 | Convidar pessoas para a Mesa ou para uma Campanha | Dono, Mestre | 1 |
| UC-12 | Aceitar convite | Usuário | 1 |
| UC-13 | Criar Campanha na Mesa | Dono | 1 |
| UC-14 | Entrar numa Campanha da Mesa | Membro | 1 |
| UC-15 | Definir papéis na Campanha (Mestre, co-Mestre, Jogador) | Mestre | 1 |
| UC-16 | Remover Membro ou sair da Mesa | Dono, Membro | 1 |
| UC-17 | Definir o link da chamada da Campanha | Mestre | 1 |
| UC-20 | Criar e editar Template (campos, grupos, fórmulas, campos de Estado) | Usuário | 1 |
| UC-21 | Copiar um Sistema para um Template próprio | Usuário | 1 |
| UC-22 | Criar Ficha a partir de um Template ou Sistema | Jogador, Mestre | 1 |
| UC-23 | Editar Ficha (com recálculo das fórmulas) | Jogador | 1 |
| UC-24 | Atualizar o Estado da Ficha em tempo real | Jogador, Mestre | 1 |
| UC-25 | Levar personagem para outra Campanha | Jogador | 1 |
| UC-30 | Criar e editar Nota (da Campanha ou da Mesa, opcionalmente ligada a uma Sessão) | Membro | 1 |
| UC-31 | Alterar a Visibilidade de uma Nota | Autor | 1 |
| UC-32 | Buscar Notas | Membro | 1 |
| UC-33 | Apagar Nota da Mesa (moderação) | Dono | 1 |
| UC-40 | Criar Sessão | Mestre | 1 |
| UC-41 | Marcar Presença | Mestre | 1 |
| UC-42 | Gerar Resumo da Sessão | Membro | 1 |
| UC-50 | Consultar as Notas pelo Assistente | Membro | 1 |
| UC-51 | Pedir sugestão de ação ao Assistente | Jogador | 1 |
| UC-52 | Ver e retomar Conversas | Usuário | 1 |
| UC-53 | Ver o próprio consumo de IA | Usuário | 1 |
| UC-60 | Conectar um Cliente MCP | Usuário | 2 |
| UC-61 | Consultar a Campanha por um Cliente MCP | Cliente MCP | 2 |
| UC-62 | Ver Apps conectados e seu uso | Usuário | 2 |
| UC-63 | Revogar um App conectado | Usuário | 2 |
| UC-64 | Criar Nota ou atualizar Estado por um Cliente MCP | Cliente MCP | 2 |
| UC-65 | Acompanhar o uso do MCP (painel interno) | Equipe | 2 |
| UC-70 | Enviar PDF ou foto de caderno para virar Notas | Mestre | 3 |
| UC-71 | Revisar e publicar Notas extraídas | Mestre | 3 |
| UC-72 | Construir a história junto com a IA | Mestre | 3 |
| UC-73 | Pedir que a IA gere uma história sozinha | Mestre | 3 |
| UC-74 | Assistente cria Nota ou altera Estado, com confirmação | Membro | 3 |
| UC-80 | Criar mapa (grid, camadas, objetos) | Mestre | 4 |
| UC-81 | Exportar mapa para impressão em escala | Mestre | 4 |
| UC-90 | Usar o mapa ao vivo numa Sessão | Mestre, Jogador | 5 |
| UC-100 | Assinar plano e ver cotas | Usuário | 6 |

## Casos de uso detalhados (MVP e MCP)

### UC-10 Criar Mesa (ou campanha avulsa)
1. O Usuário escolhe "Nova campanha" ou "Nova mesa".
2. Em **campanha avulsa**, o sistema cria uma Mesa e uma Campanha dentro dela. O Usuário vira Dono da Mesa e Mestre da Campanha, e a interface esconde o nível Mesa enquanto só houver uma Campanha.
3. Em **Mesa**, o sistema cria a Mesa vazia com o Usuário como Dono.

### UC-11 / UC-12 Convidar e aceitar
1. O Dono gera um link de convite para a Mesa, ou um Mestre gera um para uma Campanha específica.
2. Quem abre o link faz login (UC-01/02, se necessário) e aceita.
3. Convite de Campanha: a pessoa vira Membro da Mesa **e** Jogador daquela Campanha.
4. Convite de Mesa: a pessoa vira Membro e vê as Campanhas da Mesa, podendo entrar nelas (UC-14).

### UC-20 / UC-21 / UC-22 Templates, Sistemas e Fichas
1. O Usuário cria um Template com campos (texto, número, lista, booleano), grupos e fórmulas (ex.: `floor((forca - 10) / 2)`), e marca quais campos são de **Estado** (ex.: `hp_atual`, `condicoes`).
2. Para partir de um Sistema (D&D 5e SRD), o Usuário cria uma **cópia** (UC-21) e edita a cópia.
3. Ao criar uma Ficha, o usuário escolhe um Template ou um Sistema. A Ficha guarda a versão do Template usada.
4. As fórmulas são recalculadas no navegador (WASM) a cada alteração, e de novo no servidor ao salvar (ADR 0003).

### UC-24 Atualizar o Estado em tempo real
1. Durante a Sessão, o Mestre aplica 8 de dano na Ficha da Carla.
2. O servidor valida (RN-04), grava o novo `sheet_state` e publica o evento `sheet_state.updated` na Campanha.
3. Todos que estão com a Campanha aberta e podem ver a Ficha recebem o evento por **SSE**, e a tela atualiza sozinha.

### UC-30 / UC-31 Notas
1. O Membro escreve a Nota em texto rico, escolhe se ela pertence à Campanha ou à Mesa, opcionalmente liga a uma Sessão e define a Visibilidade (padrão `todos`).
2. Ao salvar, o servidor grava a Nota e enfileira um job de **indexação** para o RAG (ver [`rag.md`](rag.md)).
3. Mudar a Visibilidade **não** exige reindexar: o filtro sempre consulta a Visibilidade atual da Nota.
4. Os Membros que podem ler a Nota recebem `note.created` por SSE.

### UC-42 Gerar Resumo da Sessão
1. O Membro abre uma Sessão e pede "Resumir".
2. O servidor procura um Resumo dessa pessoa para essa Sessão. Se existir e as Notas da Sessão legíveis por ela não tiverem mudado (comparando um hash), devolve o Resumo guardado.
3. Caso contrário, monta o texto só com as Notas que a pessoa pode ler (RN-09), chama o **Haiku 5.5**, guarda o Resumo com o hash e registra os tokens (RN-11).

### UC-50 / UC-51 Consultar o Assistente
1. O Membro abre o Assistente dentro de uma Campanha e pergunta: "O que sabemos sobre o vilão de capuz?".
2. O servidor carrega a Conversa (Mongo) e chama o **Sonnet 5.5** com as ferramentas do registro (`search_notes`, `get_note`, `get_character_sheet`, `list_sessions`, `list_session_notes`).
3. O modelo chama `search_notes`, e a busca híbrida devolve só trechos legíveis pelo Membro (RN-07).
4. O modelo responde citando as Notas usadas. A resposta chega por streaming, e a Conversa é gravada com as mensagens e os tokens.
5. Em UC-51 o fluxo é o mesmo, mas o system prompt orienta a sugerir opções de ação com base na Ficha e nas Notas, e o Assistente não decide pelo Jogador.

### UC-60 Conectar um Cliente MCP
1. No Claude (ou outro cliente), o Usuário adiciona o conector `https://<domínio>/mcp`.
2. O cliente descobre o servidor de autorização e abre a tela de consentimento do RPG Note.
3. O Usuário faz login, escolhe **quais Campanhas** o app pode acessar e os scopes (`notes:read`, opcionalmente `notes:write`).
4. O cliente recebe o token, e o app aparece em "Apps conectados" (UC-62). O fluxo completo está em [`mcp.md`](mcp.md).

### UC-61 Consultar por um Cliente MCP
1. O Cliente MCP chama uma Tool, lê um Resource ou usa um Prompt.
2. O servidor valida o token, intersecta "o que o Usuário pode ler" com "as Campanhas autorizadas para esse app" e executa a mesma ferramenta do registro do Assistente.
3. A chamada é registrada em `mcp_call` (UC-65), sem o conteúdo.

## Questões em aberto

Pontos que ainda **não** foram decididos. Precisam ser resolvidos antes da issue correspondente:

- **Fichas de NPC**: o Mestre cria NPCs como Fichas da Campanha? Quem pode vê-las?
- **Jogadores veem Fichas uns dos outros?** O RN-04 só define o que o Mestre vê.
- **Exclusão de conta (LGPD)**: o que acontece com as Notas da Mesa e as Fichas de quem sai da plataforma?
- **Limites do plano gratuito** (cota de IA, número de Mesas): ficam para a revisão de monetização.
- **Regras do mapa jogável** (névoa de guerra, quem move cada peça): ficam para a Fase 5.

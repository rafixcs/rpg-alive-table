# RPG Note

Plataforma que centraliza as anotações de jogadores e mestres de RPG de mesa, com agentes de IA que consultam essas anotações e ajudam a criar histórias.

O domínio é descrito em português. No código, cada termo usa o nome em inglês indicado em _Código_.

## Language

### Organização

**Mesa**:
Um grupo de pessoas que jogam juntas. Contém uma ou mais Campanhas. Uma Mesa com uma só Campanha é tratada como uma campanha avulsa.
_Código_: `Group`
_Avoid_: Workspace, grupo, ambiente, servidor, table

**Campanha**:
Uma história jogada dentro de uma Mesa, com seus próprios Mestres, Jogadores, Fichas, Notas e Sessões.
_Código_: `Campaign`
_Avoid_: Aventura, jogo, história, workspace

**Sessão**:
Um encontro de jogo de uma Campanha, numa data (ex.: "sessão 7, quinta-feira").
_Código_: `GameSession`
_Avoid_: Partida, encontro, session (sozinho, que se confunde com sessão de login)

**Presença**:
O registro de quais Membros participaram de uma Sessão, marcado pelo Mestre.
_Código_: `Attendance`
_Avoid_: Participação, check-in

**Resumo**:
Texto gerado pela IA que conta o que aconteceu numa Sessão, a partir das Notas daquela Sessão. Cada pessoa tem o próprio Resumo, feito só com as Notas que ela pode ler.
_Código_: `SessionRecap`
_Avoid_: Sumário, ata, recap

**Dono**:
Membro da Mesa que a administra: convida pessoas, cria Campanhas e responde pela cobrança.
_Código_: `Owner`
_Avoid_: Admin

**Membro**:
Pessoa que pertence a uma Mesa. Só participa das Campanhas em que entrou.
_Código_: `Member`
_Avoid_: Usuário (quando se fala da relação com a Mesa)

**Mestre**:
Papel de quem conduz uma Campanha. É definido por Campanha, e uma Campanha pode ter vários Mestres.
_Código_: `GameMaster`
_Avoid_: GM, DM, narrador

**Jogador**:
Papel de quem participa de uma Campanha com um ou mais personagens. É definido por Campanha.
_Código_: `Player`
_Avoid_: Participante

### Anotações

**Nota**:
Texto livre escrito por um Membro, que pertence a uma Campanha ou diretamente à Mesa (ex.: regras da casa, mundo compartilhado entre Campanhas).
_Código_: `Note`
_Avoid_: Anotação, observação, documento

**Visibilidade**:
Quem pode ler uma Nota, definido pelo autor: **privada** (só o autor), **mestres** (autor + Mestres) ou **todos** (todos os Membros da Mesa ou da Campanha). O padrão é "todos". Nem o Mestre nem o Dono leem Notas privadas de outras pessoas.
_Código_: `Visibility` (`private`, `game_masters`, `everyone`)
_Avoid_: Permissão, privacidade, compartilhamento

### IA

**Assistente**:
O agente de IA do produto. É um só, para Mestres e Jogadores, e só enxerga o que a pessoa que conversa com ele pode ler.
_Código_: `Assistant`
_Avoid_: Agente, bot, chat de IA, IA (como nome próprio)

**Conversa**:
Uma troca de mensagens entre uma pessoa e o Assistente. Só quem iniciou a Conversa pode lê-la.
_Código_: `Conversation`
_Avoid_: Chat, thread, sessão

### Fichas

**Ficha**:
O registro de um personagem segundo a estrutura definida por um Template. Pertence a uma única Campanha. Levar o personagem para outra Campanha cria uma cópia ligada à Ficha original.
_Código_: `CharacterSheet`
_Avoid_: Ficha de personagem, sheet

**Estado**:
Os valores de uma Ficha que mudam durante o jogo (HP atual, mana atual, condições). Quais campos fazem parte do Estado é definido pelo Template.
_Código_: `SheetState`
_Avoid_: Status, anotações do jogador, condição (como nome do conjunto)

**Template**:
A estrutura de uma Ficha (campos, grupos e cálculos), sem ligação com nenhum personagem específico. Pode ser criado pelo usuário ou fornecido pela plataforma.
_Código_: `SheetTemplate`
_Avoid_: Modelo, schema, layout

**Sistema**:
Um sistema de RPG publicado (ex.: D&D 5e) que a plataforma oferece já pronto como Template, com regras e cálculos próprios. Não pode ser editado. Para alterar suas regras, o usuário cria uma cópia, que passa a ser um Template dele.
_Código_: `GameSystem`
_Avoid_: Sistema fixo, ruleset, system (sozinho)

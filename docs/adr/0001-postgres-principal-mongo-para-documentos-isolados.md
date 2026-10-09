# PostgreSQL como banco principal, MongoDB só para documentos isolados

O PostgreSQL guarda tudo o que participa da regra de Visibilidade ou da cobrança: Mesas, Campanhas, papéis, Sessões, Fichas, Templates, Notas, embeddings (pgvector) e a medição de tokens da IA. O MongoDB guarda apenas documentos que têm **um único dono e nenhuma regra de visibilidade cruzada**: o histórico de conversas com a IA (MVP) e, mais tarde, os Mapas. Usamos MongoDB também porque queremos praticar a tecnologia neste projeto. Por isso ele fica restrito a esses documentos, onde não arrisca o vazamento de Notas entre Jogadores e Mestres.

## Considered Options

- **Postgres + Mongo, com Fichas e Notas no Mongo.** Rejeitada. A busca da IA precisaria cruzar as permissões, que estão no Postgres, com o conteúdo no Mongo, sem transação entre os dois. Um erro nesse caminho expõe Notas do Mestre aos Jogadores.
- **Só Postgres.** Tecnicamente suficiente (JSONB cobre a estrutura flexível das Fichas), mas descartada pelo objetivo de aprendizado.

## Consequences

- Critério para dados novos: se o dado precisa respeitar a Visibilidade, entrar em consulta agregada ou ter integridade referencial, ele vai para o Postgres.
- Uma conversa com a IA guarda o texto que citou de uma Nota. Se a Nota for restrita depois, a conversa antiga continua visível para seu dono. Esse é o comportamento esperado: equivale a o Jogador ter lido a Nota antes da mudança.

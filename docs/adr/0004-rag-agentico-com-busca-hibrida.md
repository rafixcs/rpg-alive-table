# RAG agêntico com busca híbrida

O Assistente encontra informação por **RAG agêntico**: recebe ferramentas de busca (ex.: `search_notes`) e decide quando e o que buscar. A busca é **híbrida**: full-text do Postgres (dicionário `portuguese`) combinado com similaridade vetorial no pgvector, com embeddings gerados pela Voyage AI. O filtro de Visibilidade é aplicado no SQL de cada ferramenta, nunca no prompt. Além de ser a escolha técnica, RAG é uma tecnologia que queremos estudar neste projeto.

## Considered Options

- **CAG (todas as Notas legíveis no contexto, com prompt caching).** Seria mais simples e deixaria a IA ver tudo de uma vez. Rejeitada porque a Visibilidade exige um cache por pessoa, e editar Notas antigas invalida o cache. O custo cresce com a duração da campanha.
- **Híbrido CAG→RAG por tamanho (padrão dos Projects do Claude.ai).** Rejeitada por manter dois caminhos de recuperação.

## Consequences

- O pgvector e a Voyage AI entram já no MVP. Cada Nota é dividida em trechos (chunks) e indexada por um job assíncrono (River) sempre que é criada ou editada.
- Uma resposta só é tão boa quanto a busca. Perguntas que cruzam muitas Notas dependem de o modelo fazer várias buscas, por isso precisamos de casos de avaliação desde o início.

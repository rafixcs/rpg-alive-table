# Servidor MCP remoto com OAuth próprio

O RPG Note expõe um servidor MCP remoto (Streamable HTTP, rota `/mcp` no mesmo binário Go, SDK `modelcontextprotocol/go-sdk`). Assim, o usuário consulta suas Campanhas a partir de qualquer cliente MCP (Claude, ChatGPT, Cursor), e a IA roda na conta dele. O próprio RPG Note atua como servidor de autorização OAuth 2.1 (PKCE e registro dinâmico de cliente), com tokens limitados por scope (`notes:read`, `notes:write`) e por Campanha. As Tools, Resources e Prompts do MCP usam o mesmo registro de ferramentas do Assistente, com o mesmo filtro de Visibilidade. Além do valor de produto, MCP é uma tecnologia que queremos estudar.

## Consequences

- Toda chamada MCP é registrada em `mcp_call`: usuário, cliente (`clientInfo`), primitiva, nome, Campanha, duração, tamanho, resultado e número de trechos devolvidos. O conteúdo das mensagens nunca é gravado. O usuário vê esses dados em "Apps conectados", e nós num painel interno.
- O acesso ao MCP é gratuito, com limite de taxa, enquanto a monetização não for revisada.
- Nada de regra de acesso pode viver só no loop de chat do Assistente: tudo fica no registro de ferramentas, senão o MCP escaparia dela.

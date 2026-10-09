# Autenticação própria, sem senha

A autenticação é implementada no próprio backend Go, com login via OAuth (Google e Discord) e link mágico por e-mail. Não guardamos senhas. Descartamos serviços gerenciados (Clerk, Auth0, Supabase Auth) porque o custo deles cresce por usuário ativo e prende o produto ao fornecedor. Sem senha, também não há credencial nossa para vazar. O Discord entra porque é onde os grupos de RPG já se organizam.

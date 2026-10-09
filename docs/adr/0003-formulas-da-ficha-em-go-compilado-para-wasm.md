# Fórmulas da Ficha avaliadas em Go, compilado para WebAssembly

Os cálculos de uma Ficha (modificadores, CA, totais) são definidos no Template numa linguagem de fórmulas e avaliados por um único avaliador escrito em Go. O backend usa esse avaliador para validar as Fichas salvas, e o frontend usa o mesmo código compilado para WebAssembly para recalcular na hora. Escolhemos uma implementação só para que a interface e o servidor nunca deem resultados diferentes.

## Considered Options

- **Dois avaliadores, um em TS e outro em Go.** Rejeitada pelo risco de divergência entre eles.
- **Calcular só no frontend.** Rejeitada porque o backend aceitaria qualquer valor enviado, e a IA leria Fichas sem validação.

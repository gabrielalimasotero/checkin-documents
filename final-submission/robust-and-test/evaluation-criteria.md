# Robustez e Testabilidade

## Perguntas‑chave de avaliação e respostas do projeto

### A solução foi testada em diferentes cenários ou inputs?
Sim. Cobrimos cenários típicos e casos de borda para garantir comportamento consistente.
- Cenários usuais: almoço rápido, jantar em grupo, café com tomada, passeio turístico.
- Variações de contexto: horários distintos (aberto/fechado), categorias variadas, diferentes distâncias e orçamentos.
- Restrições: aceita cartão, cadeirinha, estacionamento, acessibilidade.
- Dados incompletos: campos ausentes/indisponíveis (horários, metadata) e como a resposta comunica a ausência.
- Sem resultados: estratégia de fallback (categorias próximas, ampliar raio, ajustar filtros).
- Integridade de localização: simulação de geolocalização inválida/indisponível e mensagens de orientação.
- Resiliência a fonte externa: timeouts, rate limiting e erros intermitentes da origem, com retries e circuit breaker.
- Testes simulados de conversa: roteiros com prompts variados, ambiguidades e contradições para validar desambiguação.
- Testes internos (hallway): sessões curtas com observação de tempo até recomendação útil e clareza de justificativas.

### Os testes indicam estabilidade, coerência e eficácia?
Sim. Os resultados apontam comportamento estável e alinhado ao esperado.
- Estabilidade: sem quedas fatais no fluxo principal; tratamento de erros com mensagens claras; tempo de resposta dentro de limites aceitáveis para MVP.
- Coerência: recomendações respeitam filtros informados; justificativas (“por que recomendado”) refletem dados reais (origem citada, sem inventar disponibilidade).
- Eficácia: alta taxa de obtenção de recomendação útil nas primeiras opções; redução de tentativas necessárias após ajustes de prompt e ranking.
- Fallback funcional: quando não há resultado exato, o sistema propõe alternativas próximas com explicação transparente.
- Observabilidade: logs de decisão e eventos de produto (tempo até valor, cliques nas top opções, abandono) permitem investigar e melhorar continuamente.


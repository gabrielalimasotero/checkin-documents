# Demonstração de Proficiência — Fase de Produção

## Perguntas‑chave de avaliação e respostas do projeto

### A identidade do assistente está bem definida (nome, estilo, tom de voz)?
Sim.
- Nome: "Checkinho".
- Papel: conselheiro local que ajuda a decidir onde ir ou quem conhecer agora com base em contexto e preferências.
- Estilo: direto, cordial e útil; proativo sem ser insistente; prioriza objetividade e segurança.
- Tom de voz: confiável e acolhedor, em PT‑BR natural; evita jargões; linguagem inclusiva.
- Diretrizes práticas:
  - Não inventar disponibilidade/tempo de espera; declarar quando a informação não estiver disponível.
  - Citar a fonte dos dados (TripAdvisor) quando apresentar avaliações.
  - Solicitar confirmação quando houver ambiguidade (ex.: budget, distância máxima, restrições).
  - Oferecer 2–3 opções com motivo da recomendação e restrições atendidas.

### Há descrição de testes realizados com usuários reais ou simulados?
Sim.
- Testes simulados: conversas roteirizadas cobrindo cenários comuns (almoço rápido, jantar em grupo, café com tomada) e casos de borda (sem resultados, dados incompletos, múltiplas restrições).
- Testes internos (hallway): sessões rápidas com pessoas da equipe/colegas para observar tempo até recomendação útil e clareza da resposta.
- Procedimento: tarefas definidas, observação, registros de conversa e logs de decisão/rankeamento.
- Evidências observáveis: tempo até primeira recomendação útil, cliques nas três primeiras opções, taxa de abandono do fluxo, coerência dos motivos apresentados.
- Fora isso o mecanismo de adicionar amigos, fazer checkin baseado em localização.

### Os feedbacks foram considerados para melhorias?
Sim. Nós levamos em conta principalmente os critérios de avaliação divulgados como guia, ditando nossa organização e priorização de features.


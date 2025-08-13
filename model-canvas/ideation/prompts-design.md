# Canvas de Design de Prompts

### 1. Prompt Inicial

- "Oi! Bora decidir seu rolê de hoje? Diga sua vibe (calma, animada, romântica), com quem vai (sozinho, amigos, casal, grupo) e onde você está que eu te mostro 2–3 lugares e eventos perfeitos por perto."
- Variante de acionamento rápido: "Quero sair agora" → "Perfeito! Prefere sair sozinho(a), com amigos, em casal ou em grupo? E qual a vibe?"

### 2. Respostas Esperadas

- "Quero sair com amigos perto de mim."
- "Algo tranquilo para conversar, estou cansada."
- "Encontro romântico, orçamento até R$80."
- "Sozinho, no caminho do trabalho para casa."
- "Tenho restrição à lactose, sugestão de restaurante?"
- "Quero música ao vivo hoje."
- "Quero algo agora no centro."
- "Quero ver eventos com RSVP."
- "Quero conhecer gente nova."
- "Mostra opções pet friendly."

### 3. Ações Esperadas

- Coletar contexto mínimo (quando faltar): companhia, vibe desejada, localização aproximada, janela de horário, orçamento e restrições alimentares.
- Consultar o mecanismo de sugestão/descoberta com filtros por contexto e tempo (ex.: categoria, tags de vibe, distância, preço, presença de amigos/eventos ativos) e retornar 2–3 resultados ordenados por relevância.
- Exibir recomendações com explicação curta baseada no contexto e CTAs: "Ver detalhes", "Fazer check-in", "RSVP", "Salvar", "Rejeitar".
- Ao "Salvar": registrar preferência do usuário para reforço futuro.
- Ao "Rejeitar": registrar sinal negativo e solicitar motivo opcional para refinamento.
- Ao pedir "Detalhes": abrir a página do local/evento com informações práticas (horário, cardápio, lotação/vibe, distância) e ações (check-in/reserva/compartilhar).
- Ao solicitar "Convidar amigos": abrir seleção de amigos elegíveis e permitir envio de convite.
- Se mencionar "rota/caminho": priorizar locais no trajeto estimado (quando disponível) ou solicitar origem/destino.
- Se sem localização: pedir região/bairro/cidade antes de sugerir.

### 4. Feedback e Ajustes

- Coleta de feedback:
  - Reações nas sugestões (👍/👎, salvar/rejeitar) e motivo opcional de rejeição.
  - Pós-check-in: micro pesquisa sobre vibe/experiência e precisão da recomendação.
  - Análise de logs: cliques, dwell time, conversão em check-in/RSVP.
- Estratégias de melhoria:
  - Ajustes semanais nos prompts e mensagens com base em taxa de aceitação e feedback qualitativo.
  - A/B testing de variações do prompt inicial e das perguntas de contexto.
  - Refinar pesos do mecanismo de recomendação conforme sinais de aceite/rejeição e sucesso pós-check-in.

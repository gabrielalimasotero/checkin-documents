# Canvas de Ideação de Soluções

### 1. Problema a Ser Resolvido

- Decidir “para onde sair agora” é lento e fragmentado. Usuários precisam cruzar vários apps (mapas, redes, reviews) sem contexto atual (vibe, fila, presença de amigos), causando frustração, baixa assertividade e desistência do rolê.

### 2. Ideias de Solução

- Assistente de IA conversacional (LLM) para captar contexto (companhia, vibe, orçamento, localização) e sugerir 2–3 opções explicadas.
- Motor de recomendação contextual híbrido (regras + sinais comportamentais + re-ranking via LLM) com explicabilidade.
- Tags de “vibe” em tempo real (crowdsourcing pós-check-in; lotado/calmo/música alta/etc.).
- Sistema de convites e RSVP com sugestões automáticas de lugares compatíveis com o grupo.
- Destaques curados (Todos, Promoções, Restaurantes, Bares, Shows, Eventos) atualizados por horário/local.
- Explorar social (Lugares, Grupos, Pessoas) com privacidade controlada.

### 3. Benefícios Esperados

- Redução do tempo até a decisão e menor frustração.
- Maior assertividade e satisfação pós-visita; aumento de check-ins/RSVPs.
- Engajamento social mais alto (convites, mensagens, notificações relevantes).
- Valor para estabelecimentos (visibilidade qualificada, gestão de fluxo).
- Base de dados rica para melhorar recomendações e curadoria.

### 4. Viabilidade Técnica

- Assistente de IA (LLM): Alta. Uso de APIs (OpenAI), prompts e guardrails; integração via FastAPI.
- Recomendação contextual híbrida: Média/Alta. Regras iniciais (horário/distância/vibe) + sinais simples, re-ranking por LLM.
- Tags de vibe em tempo real: Alta. Coleta pós-check-in com decaimento temporal e validação cruzada.
- Convites e RSVP: Alta. CRUD padrão com lógica de confirmação; baixo risco técnico.
- Destaques curados: Alta. Jobs por horário/local + filtros; heurísticas no MVP.
- Explorar social: Média. Requer controles de privacidade e busca eficiente, porém CRUD + paginação.

### 5. Priorização de Soluções

- Alta prioridade: Assistente de IA conversacional; Recomendação contextual; Tags de vibe;
- Média prioridade: Destaques curados; Explorar social (Lugares, Grupos, Pessoas), Convites e RSVP.
- Baixa prioridade: Assistente por voz; Matchmaking avançado; Roteiros automáticos; Marketplace/AR.
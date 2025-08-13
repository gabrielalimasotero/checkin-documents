# Canvas de Prototipagem Rápida

### 1. Ideia de Solução

- Assistente conversacional e feed "For You" para decidir onde sair agora, com curadoria por contexto (horário, localização, companhia, vibe, restrições), check-in/RSVP e explicações curtas das recomendações.

### 2. Objetivo do Protótipo

- Validar redução do tempo até a decisão para ≤ 2 min no fluxo "Quero sair".
- Alcançar ≥ 70% de relevância percebida nas sugestões.
- Confirmar usabilidade do check-in/RSVP sem ajuda em > 85% dos testes.
- Medir adoção de salvar/rejeitar e impacto no refinamento das sugestões.

### 3. Recursos Necessários

- Ferramentas:
  - Lovable (protótipos e navegação)
  - React + Vite (protótipo funcional web/PWA)
  - FastAPI stub/mock (sugestões, check-in, eventos)
  - Supabase (auth/dados mock) ou JSON estático para simulação
- Equipe:
  - UX/UI (1), Frontend (1), Backend (1), Produto/Pesquisa/Documentação (1), Banco de Dados (1), IA (1)
- Dados:
  - Catálogo sintético de lugares (nome, tags de vibe, preço, distância)
  - Eventos fictícios com RSVP
  - Perfis/personas para recrutamento
- Outros:
  - Roteiros de teste, questionários SUS/NPS, ferramenta de gravação remota
  - Escopo do protótipo (telas):
    - Login
    - Home: Network, For You, Destaques, Convites, Mensagens, Notificações
    - Destaques: Todos, Promoções, Restaurantes, Bares, Shows, Eventos
    - Check-in: Geral, Ativo, Histórico
    - Assistente virtual (overlay/modal)
    - Explorar: Lugares, Grupos, Pessoas
    - Perfil: Configurações

### 4. Cronograma de Desenvolvimento

- Semana 1: Preparar dados e wireframes; definir roteiro de testes
- Semana 2–3: Protótipo interativo (Lovable/React); simular mecanismo de sugestão; telas de detalhes e check-in/RSVP
- Semana 4: Testes moderados e não moderados; ajustes de UX; iteração nos prompts do assistente
- Semana 5: Piloto com +-6 usuários; consolidação de métricas e relatório

### 5. Métricas de Sucesso

- Tempo até decisão: ≤ 2 min (p50) e ≤ 3 min (p90)
- Aceitação de sugestões (salvar/seguir): ≥ 60%
- Conversão para check-in/RSVP: ≥ 40% entre sessões com recomendação
- Relevância percebida das sugestões: ≥ 4/5
- SUS ≥ 70; NPS ≥ 40
- Erros críticos em usabilidade: ≤ 5%

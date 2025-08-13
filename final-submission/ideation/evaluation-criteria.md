# Fase de Ideação

## Perguntas‑chave de avaliação e respostas do projeto

### Os objetivos do projeto são claros, mensuráveis e realistas?
Sim. Definimos objetivos a partir de valor para o usuário e para o negócio, com indicadores observáveis e escopo viável para o MVP.
- Clareza: objetivos articulados em torno da jornada crítica “onde ir agora”, reduzindo atrito na descoberta e aumentando confiança na decisão.
- Mensuráveis: definimos indicadores observáveis como tempo até valor (primeira recomendação útil), taxa de ativação do primeiro check‑in, adoção de filtros de preferência, cliques em recomendações e satisfação pós‑visita (ex.: CSAT/NPS qualitativo nesta etapa).
- Realistas: foco em um conjunto enxuto de cenários (descoberta local + filtros essenciais), uso de uma fonte de dados inicial (TripAdvisor) e arquitetura preparada para evolução, evitando metas numéricas rígidas antes da validação.
- Alinhamento: objetivos conectados às necessidades da persona principal e às restrições técnicas identificadas.

### O design de prompt é bem elaborado, alinhado com a persona e os objetivos?
Sim. O design de prompt foi estruturado para refletir a persona “Explorador Urbano” e otimizar a tomada de decisão rápida e confiável.
- Alinhamento com a persona: captura intenção (ex.: jantar rápido/ocasião social), preferências e restrições (ex.: aceita cartão, cadeirinha, estacionamento) e contexto (horário, localização, categoria).
- Contexto no prompt: inclui geolocalização, janela de horário, categoria/estilo do lugar, preferências históricas/sinais de interesse e políticas de conteúdo (não inferir dados sensíveis; não alucinar disponibilidade).
- Saída estruturada: orienta a resposta em formato consistente (ex.: lista com campos nome, distância, motivo da recomendação, restrições atendidas), facilitando ranking e apresentação na interface.
- Confiabilidade: instruções para citar a fonte dos dados e não inventar informações (ex.: tempo de espera) quando ausentes; estratégia de fallback.
- Qualidade: uso de exemplos (few‑shot) e casos de borda para garantir consistência; revisão com base em feedback de testes iniciais.




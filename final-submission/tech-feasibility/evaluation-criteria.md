# Viabilidade Técnica

## Perguntas‑chave de avaliação e respostas do projeto

### A solução é tecnicamente viável e implementável com os recursos descritos?
Sim. A solução foi planejada para um MVP com tecnologias consolidadas e baixo atrito operacional.
- Arquitetura: frontend (React/Vite) + backend (FastAPI/Python) + banco gerenciado (Supabase/Postgres).
- Integração de dados: fonte inicial TripAdvisor, com normalização e camadas de adaptação para futuras fontes.
- Infra/entrega: Docker, CI/CD (GitHub Actions) e deploy em PaaS (ex.: Vercel/Render/Fly/Heroku) com HTTPS e variáveis de ambiente seguras.
- Observabilidade: logs estruturados, métricas básicas (latência, erros), tracing amostral e alertas essenciais.
- Escopo MVP: foco em descoberta local, filtros essenciais e recomendações contextualizadas; evita complexidade prematura.
- Risco/mitigação: dependência de um provedor mitigada por adaptadores; time‑outs, retries e fallbacks; código modular para evolução.

### O uso de LLMs está bem justificado tecnicamente?
Sim. O LLM é usado onde agrega valor sem substituir dados de fato verificados.
- Justificativa de uso:
  - Interação em linguagem natural para capturar intenção, preferências e restrições rapidamente.
  - Composição/explicação de recomendações (rationale) a partir de dados estruturados, melhorando confiança e UX.
  - Desambiguação e coleta incremental de contexto (ex.: orçamento, distância, ocasião).
- Limites e salvaguardas:
  - O LLM não inventa dados: respostas se baseiam em fontes confiáveis (TripAdvisor) e sinalizam ausência de informação.
  - Guardrails de prompt: políticas para não alucinar disponibilidade/tempo de espera; citação de fonte; formato de saída estruturado.
  - Estratégia de fallback: quando o contexto for insuficiente, pedir confirmação; quando não houver resultados, sugerir categorias próximas.
- Operação e custo:
  - Chamadas curtas e cacheáveis (prompts enxutos com campos estruturados), priorizando latência baixa.
  - Possibilidade de switch de provedores/modelos sem refatoração ampla (abstração por interface).
  - Monitoramento de qualidade (taxa de correção manual, coerência do “por que recomendado”).


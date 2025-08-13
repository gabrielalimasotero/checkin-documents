# Arquitetura da Solução

## Perguntas‑chave de avaliação e respostas do projeto

### A arquitetura está documentada com clareza? (pode incluir diagramas)
Sim. A documentação cobre visão de alto nível e detalhamento técnico, de forma rastreável:
- Diagramas C4: níveis de Contexto, Contêiner e Componente, alinhados ao código e aos limites de módulo.
- Contratos de API: especificação OpenAPI/Swagger gerada automaticamente no backend (FastAPI), descrevendo endpoints, modelos e erros.
- Fluxo de dados e ETL: diagrama de ingestão (TripAdvisor → normalização → armazenamento), incluindo políticas de atualização e validações.
- Modelo de dados: entidades principais (estabelecimento, avaliações, metadados, preferências do usuário) e relacionamentos.
- Arquitetura de recomendações: componentes de ranking/filtragem e pontos de integração com o LLM para rationale e desambiguação.
- Deploy/Operação: diagrama de ambientes (dev/staging/prod), CI/CD, observabilidade (logs/métricas/tracing) e gestão de segredos.
- Decisões arquiteturais (ADRs): registro das principais escolhas e seus trade‑offs.

### Justifica‑se o uso de pipelines, ferramentas ou frameworks específicos?
Sim. As escolhas foram guiadas por velocidade de entrega do MVP, simplicidade operacional e qualidade.
- FastAPI (backend): tipagem estática com Pydantic, performance adequada, geração automática de OpenAPI e rapidez de desenvolvimento.
- React + Vite (frontend): iteração rápida, ecossistema maduro e fácil integração com componentes reutilizáveis.
- Banco gerenciado (Supabase/Postgres): acelera entrega, autenticação e armazenamento com baixo overhead operacional.
- Docker + PaaS (Vercel/Render/Fly/Heroku): empacotamento reprodutível e deploy simples; foco no produto em vez de infra.
- CI/CD (GitHub Actions): pipelines de build/testes/lint e deploy automatizado; gates mínimos antes de produção.
- Pipeline de dados (ETL): ingestão incremental, normalização e validação para garantir qualidade e frescor dos dados consumidos pelo app.
- LLM (rationale e interface natural): usado para entender intenção e explicar recomendações; com guardrails no prompt (não inventar disponibilidade, citar fonte) e estratégia de fallback.
- Observabilidade: logs estruturados, métricas básicas e alertas para reduzir MTTR e orientar melhorias.
- Trade‑offs: priorizamos ferramentas com curva de aprendizado baixa e boa integração entre si; mantemos camadas de adaptação para troca futura de provedores (LLM, dados, PaaS) sem refatoração ampla.


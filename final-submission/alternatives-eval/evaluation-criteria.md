# Avaliação de Alternativas

## Perguntas‑chave de avaliação e respostas do projeto

### Outras abordagens foram consideradas?
Sim. Mapeamos alternativas em cada decisão relevante e registramos trade‑offs.
- Fontes de dados: TripAdvisor (escolhida inicialmente) vs. Google Places, Foursquare, Yelp, OSM. Critérios: cobertura, qualidade, termos de uso, custo e facilidade de integração.
- Estratégia de recomendação: regras determinísticas + ranking leve (escolhida) vs. modelo supervisionado próprio (posterior) vs. LLM no caminho crítico (descartado para dados factuais). Critérios: previsibilidade, custo, latência, explicabilidade.
- Uso de LLM: fora do caminho crítico para intenção/explicação (escolhida) vs. geração completa da resposta sem estrutura (descartada). Critérios: controle de alucinação, rastreabilidade de fonte, formato estruturado.
- Backend: FastAPI (escolhido) vs. Node/Nest. Critérios: rapidez de desenvolvimento, tipagem/dados (Pydantic), OpenAPI automático.
- Banco/infra: Supabase/Postgres gerenciado (escolhido) vs. auto‑gerenciado. Critérios: time‑to‑MVP, segurança/operacional, custo.
- Deploy: PaaS (Vercel/Render/Fly/Heroku) (escolhido) vs. IaaS. Critérios: simplicidade operacional, velocidade, suporte a rollback.
- App: Web responsivo (escolhido) vs. mobile nativo no MVP. Critérios: alcance, velocidade de entrega, manutenção.

### As decisões foram tomadas com base em critérios sólidos (custo, risco, tempo, impacto)?
Sim. Usamos critérios explícitos para priorizar o caminho de menor risco e maior impacto no MVP.
- Tempo: reduzir time‑to‑value privilegiando ferramentas com alto grau de automação e integração nativa.
- Custo: optar por serviços gerenciados gratuitos/baixo custo no início; controlar custos de LLM com prompts curtos e cache.
- Risco: evitar lock‑in forte com camadas de adaptação (fontes de dados, LLM, PaaS); plano de fallback e testes de contrato.
- Impacto no usuário: privilegiar previsibilidade, latência baixa e explicabilidade das recomendações.
- Qualidade dos dados: normalização/validação e estratégia incremental para atualizar dados com segurança.
- Operação: observabilidade desde o início (logs, métricas, alertas) e deploy automatizado com rollback simples.

Exemplos de decisões e trade‑offs:
- TripAdvisor primeiro: melhor relação cobertura/qualidade/esforço; risco mitigado por adaptadores para futuras fontes.
- FastAPI + Supabase: entrega rápida com OpenAPI automático e banco gerenciado; menor carga operacional.
- LLM para intenção/explicação: mantém dados factuais fora do LLM; reduz alucinações e custo; saída estruturada para UX consistente.


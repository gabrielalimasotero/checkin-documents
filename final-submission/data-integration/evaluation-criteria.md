# Integração com Dados

## Perguntas‑chave de avaliação e respostas do projeto

### Os dados foram preparados de forma adequada?
Sim. As informações obtidas do TripAdvisor passaram por um processo de extração, transformação e carga (ETL), sendo organizadas em formatos estruturados JSON e CSV, além de já estarem disponíveis no banco de dados para uso direto pelo sistema. Durante essa preparação, foram realizados tratamentos para dados ausentes, padronização de campos, unificação de categorias e verificação de integridade, assegurando que os dados estejam consistentes e prontos para utilização na aplicação.

### Há estratégias claras para alimentar e manter o sistema atualizado?
Sim. Estratégias definidas para atualização contínua e confiável:
- Agendamento e incremental: rotinas de ingestão incremental (ex.: cron) com janelas de atualização definidas (ex.: diária/horária) e capacidade de reprocessamento (backfill).
- Upsert idempotente: deduplicação por chaves estáveis, merge de registros, tratamento de soft‑delete e detecção de mudanças por timestamps/hashes.
- Validação e qualidade: validação de esquema, checks de integridade (campos obrigatórios, ranges, geolocalização válida), regras de normalização e alertas quando houver degradação de qualidade.
- Observabilidade do pipeline: logs, métricas e alertas para falhas de ingestão, taxas de erro por fonte e tempo de processamento.
- Resiliência a fontes externas: retries com backoff, rate limiting, circuit breaker e filas para amortecer indisponibilidades temporárias.
- Cache e invalidação: TTL e políticas de invalidação para garantir frescor sem sobrecarregar a origem; recomputo de campos derivados quando necessário.
- Governança e mudanças: versionamento de esquema, migrações controladas e compatibilidade retroativa quando possível.
- Conformidade e licenças: revisão periódica de termos de uso/limites da fonte e registro de proveniência dos dados no catálogo.


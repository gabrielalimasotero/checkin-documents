# Integração com Dados

## Perguntas‑chave de avaliação e respostas do projeto

### Os dados foram preparados de forma adequada?
Sim. As informações obtidas do TripAdvisor passaram por um processo de extração, transformação e carga (ETL), sendo organizadas em formatos estruturados JSON e CSV, além de já estarem disponíveis no banco de dados para uso direto pelo sistema. Durante essa preparação, foram realizados tratamentos para dados ausentes, padronização de campos, unificação de categorias e verificação de integridade, assegurando que os dados estejam consistentes e prontos para utilização na aplicação.

## Obtenção dos dados para popular o banco via Webscraping no TripAdvisor

Descreveremos o processo de coleta de dados de restaurantes de Recife no TripAdvisor, utilizando uma combinação de scripts em Python, Bash e JavaScript, além do uso da extensão Tampermonkey para automatizar a extração.

1. Primeira Implementação com Selenium
A primeira tentativa foi utilizar Selenium em Python para simular um navegador e coletar os dados diretamente das páginas. Porém, o TripAdvisor detectou facilmente a automação, o que inviabilizou essa abordagem inicial.

2. Uso do Console do Navegador para Extração Manual
Como alternativa, passamos a utilizar scripts de JavaScript diretamente no console do navegador para extrair os links das páginas de restaurantes. Assim, conseguimos gerar um arquivo de texto com as URLs dos restaurantes de Recife.

3. Automatização via Bash e Tampermonkey
Para contornar a detecção automatizada e tornar o processo repetitivo, utilizamos um script em Bash que abria o Google Chrome com as URLs coletadas. Usando a extensão Tampermonkey, injetamos um script que rodava ao carregar a página, extraindo automaticamente os dados de cada restaurante.

4. Coleta de Features Específicas
Durante o processo, foi possível coletar informações detalhadas como acessibilidade (cadeirinha para bebê), estacionamento, se o restaurante aceita pets, tipos de cartão aceitos, entre outros. Essas informações foram agrupadas e consolidadas em um arquivo CSV ao final.

5. Desafios e Considerações Finais
O TripAdvisor apresenta desafios por não permitir facilmente esse tipo de scraping, o que exigiu essa abordagem mais criativa. Sem essa solução híbrida, não teríamos conseguido entregar os dados.


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


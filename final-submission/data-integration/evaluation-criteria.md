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
O TripAdvisor apresenta desafios por não permitir facilmente esse tipo de scraping, o que exigiu essa abordagem mais criativa. Sem essa solução híbrida, não teríamos conseguido entregar os dados. Vale ressaltar que essa forma de obtenção de dados não é a ideia do app.


### Há estratégias claras para alimentar e manter o sistema atualizado?
Sim. No CheckIN, os estabelecimentos devem se cadastrar oficialmente no app e manter seus dados atualizados dentro da plataforma. Estratégias definidas:
- Entrada oficial de dados: cadastro e atualização realizados pelos próprios estabelecimentos via app/portal (sem scraping). Campos obrigatórios validados no fluxo.
- Ingresso de novos cadastros: onboarding guiado com verificação básica (e‑mail/telefone), termos e política de uso; aprovação moderada quando necessário.
- Atualizações cadastrais recorrentes: lembretes e exigências periódicas (ex.: confirmação trimestral de horário/capacidade/promos); bloqueio de destaque para perfis desatualizados.
- Upsert idempotente: deduplicação por CNPJ/ID oficial, merge de registros, tratamento de soft‑delete e detecção de mudanças por timestamps/hashes.
- Validação e qualidade: validação de esquema, checks de integridade (campos obrigatórios, ranges, geolocalização válida), normalização de categorias e alertas de degradação.
- Observabilidade: logs, métricas e alertas para cadastros pendentes, taxas de atualização por estabelecimento e falhas no fluxo de edição.
- Cache e invalidação: TTL e políticas de invalidação para listas (ex.: destaques por horário) e recomputo de campos derivados após edição.
- Governança: versionamento de esquema, migrações controladas, trilha de auditoria por estabelecimento, registro de proveniência interna.


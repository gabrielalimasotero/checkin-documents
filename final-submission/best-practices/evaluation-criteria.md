# Boas Práticas

## Perguntas‑chave de avaliação e respostas do projeto

### Há uso de boas práticas de versionamento, modularização, e reusabilidade?
Sim.
- Versionamento: Git com branches de feature, pull requests com revisão, mensagens padronizadas (ex.: Conventional Commits), tags para releases e CHANGELOG para rastrear mudanças relevantes.
- Modularização: camadas separadas (frontend, backend, pipelines de dados) e limites de módulo claros (ex.: adaptadores de fontes de dados, serviços de recomendação, camadas de API e UI). Contratos definidos entre módulos e configuração por ambiente.
- Reusabilidade: componentes de UI reutilizáveis, hooks e utilitários no frontend; serviços e repositórios no backend; prompts e esquemas de saída padronizados; funções comuns para validação/normalização de dados.
- Qualidade contínua: lints e formatação automatizada, testes por camada e políticas mínimas de revisão antes de merge.

### O projeto aplica conceitos como MLOps, DevOps ou boas práticas de entrega?
Sim.
- DevOps/Entrega: CI/CD com build, lints, testes e empacotamento em Docker; deploy automatizado em ambientes separados (staging/produção) com variáveis de ambiente seguras e rollback simples.
- Observabilidade: logs estruturados, métricas básicas (latência, taxa de erro, throughput), tracing amostral e alertas para falhas críticas.
- Segurança: gestão de segredos, CORS adequado, HTTPS, varredura de dependências e princípios de menor privilégio.
- MLOps/LLMOps: versionamento de prompts e parâmetros, harness de avaliação com conversas simuladas, guardrails para evitar alucinação (citar fonte, não inventar disponibilidade) e monitoramento de qualidade das respostas (coerência do “por que recomendado”).
- Dados e governança: validação de esquema no pipeline, controles de qualidade (campos obrigatórios, ranges, geolocalização), proveniência registrada e versionamento de mudanças de esquema.


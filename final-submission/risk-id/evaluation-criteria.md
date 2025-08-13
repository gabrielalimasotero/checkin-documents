# Identificação de Riscos

## Perguntas‑chave de avaliação e respostas do projeto

### Foram identificados possíveis vieses, exclusões ou riscos éticos?
Sim. Identificamos riscos e definimos salvaguardas e monitoramento contínuo.

- Vieses de fonte de dados (TripAdvisor):
  - Representatividade desigual (bairros centrais/populares super‑representados; cidades menores com gaps).
  - Viés de auto‑seleção em avaliações (tendência a extremos; risco de manipulação/reviews falsos).
  - Mitigação: plano multi‑fontes via adaptadores; deduplicação/normalização; detecção de anomalias; sinalização de confiança.

- Exclusões e acessibilidade:
  - Falta de cobertura para restrições específicas (acessibilidade, dieta, família com crianças).
  - Barreiras de idioma e letramento digital.
  - Mitigação: campos e filtros inclusivos (ex.: acessibilidade, cadeirinha); textos claros em PT‑BR; ícones e rótulos acessíveis; coleta de feedback de grupos sub‑representados.

- Privacidade e segurança (LGPD):
  - Risco ao usar geolocalização e preferências sem consentimento explícito.
  - Mitigação: minimização de dados; consentimento granular e opt‑out; retenção limitada; geolocalização com precisão reduzida (geohash/raio) quando não necessário; armazenamento seguro e segregação de segredos.

- Ética no uso de LLM:
  - Alucinação de fatos (ex.: disponibilidade/tempo de espera), linguagem imprópria ou enviesada.
  - Mitigação: guardrails no prompt (não inventar; citar fonte); validação contra dados estruturados antes de exibir; filtros de linguagem tóxica; exemplos (few‑shot) para neutralidade.

- Transparência e explicabilidade:
  - Ranking pode privilegiar locais populares ou próximos, penalizando diversidade.
  - Mitigação: mostrar “por que recomendado”; controles de usuário (ajustar raio/critério); diversidade mínima na lista; rótulo claro para ofertas/patrocínio.

- Conteúdo patrocinado e conflitos de interesse:
  - Risco de degradar relevância e confiança.
  - Mitigação: políticas de relevância mínima; rotulagem explícita; isolamento do algoritmo de ranking orgânico; auditorias periódicas.

- Conformidade com termos de uso/licenças:
  - Risco em raspagem/uso indevido de dados de terceiros.
  - Mitigação: respeito a ToS/APIs oficiais; registro de proveniência; auditoria de licenças; fallback para dados próprios/cadastrados na plataforma.

- Monitoramento e governança contínuos:
  - Métricas de fairness (distribuição por bairro/categoria), taxa de reclamações/appeals, correções manuais.
  - Canais de denúncia e revisão; ADRs documentando decisões éticas; revisões periódicas de risco.


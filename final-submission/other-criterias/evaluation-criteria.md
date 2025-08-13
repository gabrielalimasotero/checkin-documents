# Outros Critérios

## Perguntas‑chave de avaliação e respostas do projeto

### [Estratégias de Mitigação] O projeto propõe mecanismos para mitigar os riscos identificados?
Sim. As principais salvaguardas estão definidas e alinhadas aos riscos mapeados.
- Multi‑fontes via adaptadores para reduzir dependência e vieses; deduplicação/normalização de dados.
- Privacidade/LGPD: consentimento granular, minimização de dados, retenção limitada, segredos protegidos.
- Guardrails de LLM: não inventar disponibilidade; citar fonte; saída estruturada; filtros de linguagem.
- Fallbacks: quando não houver resultados exatos, ampliar raio/categoria e comunicar claramente as limitações.
- Observabilidade: logs estruturados, métricas e alertas para incidentes e degradação de dados.
- Governança: ADRs, revisões periódicas de risco e canais de denúncia/appeals.
- Transparência de patrocínio: rotulagem explícita e relevância mínima para conteúdo pago.

### [Transparência] Há preocupação em comunicar claramente como a IA funciona para os usuários?
Sim. O produto comunica as bases das recomendações e limitações.
- Justificativa “por que recomendado”, com critérios (aderência, distância, popularidade) e restrições atendidas.
- Mensagens claras quando a informação não estiver disponível; explicitar suposições/fallbacks.
- Políticas de uso de dados e consentimento visíveis; opção de revisar/ajustar preferências.

### [Sustentabilidade] A solução considera efeitos de longo prazo, manutenção e impacto social?
Sim. A arquitetura e o roadmap contemplam sustentabilidade técnica e social.
- Manutenibilidade: módulos com contratos claros, ADRs, versionamento de esquemas e CI/CD.
- Custos e operação: PaaS e serviços gerenciados para reduzir overhead; uso parcimonioso de LLM (prompts enxutos, cache).
- Impacto social: apoio ao comércio local; filtros inclusivos (acessibilidade/família); linguagem clara e não excludente.
- Escalabilidade responsável: prontidão para multi‑fontes, internacionalização e métricas para evitar vieses sistêmicos.

### [Estrutura e Fluidez] A documentação está bem organizada, com começo, meio e fim?
Sim. A documentação segue uma narrativa consistente por pastas de submissão final.
- Estrutura por temas (ideação, imersão, produção, validação e eixos transversais).
- Cada pasta contém perguntas‑chave respondidas objetivamente e artefatos vinculados.
- Decisões e trade‑offs registrados (ADRs); referências e evidências anexadas quando aplicável.

### [Linguagem e Comunicação] A linguagem é acessível para um público técnico e de negócios?
Sim. Utilizamos linguagem clara em PT‑BR, com precisão técnica quando necessário.
- Resumos executivos e seções técnicas complementares.
- Glossário de termos e rótulos consistentes na interface.
- Evitamos jargões desnecessários e explicitamos suposições.

### [Evidências] Foram incluídos protótipos, prints, outputs ou links funcionais?
Sim. Evidências práticas acompanham as respostas.
- Wireframes/prints de telas e fluxos principais.
- Exemplos de respostas do assistente com justificativas e citações de fonte.
- Amostras de dados (JSON/CSV) e consultas/outputs do banco.
- Roteiros e registros de testes simulados/"hallway".

### [Referências] Foram usadas referências relevantes (papers, artigos, docs de APIs, etc.)?
Sim. As principais decisões citam referências técnicas, de produto e literatura correlata.
- Documentação de APIs e termos de uso das fontes de dados.
- Referências de frameworks e ferramentas (FastAPI, React/Vite, Supabase/Postgres, CI/CD).
- Boas práticas de arquitetura (C4, ADRs, DDD) e recomendações.
- Materiais sobre LLMs/LLMOps, mitigação de vieses e transparência em sistemas de IA.

Estudos pra embasar o projeto:

1) Atendimento melhora quando há avaliação
- Estudos indicam que saber que se está sendo avaliado ou receber feedback frequente gera mudanças positivas no comportamento dos prestadores de serviço.
- Efeito Hawthorne e apreensão de avaliação mostram que a simples consciência de estar sendo observado leva a ajustes no comportamento.
- Pesquisas em restaurantes e plataformas online revelam que avaliações impactam diretamente recompensas financeiras e reputação, criando incentivo para manter padrões elevados de atendimento.
- Modelos como SERVQUAL comprovam que medir e comunicar a percepção de qualidade ajuda a alinhar expectativas e promover melhoria contínua.
- Feedback estruturado e análise de avaliações online possibilitam correções rápidas e aprimoramento constante.
- Impacto para o CheckIn: O sistema de avaliação integrado funcionaria como motor para elevação da qualidade do atendimento nos estabelecimentos presencialmente, estimulando melhores atendimentos e organização.

2) Vida social, bem‑estar e saúde mental
- A literatura científica mostra que conexões sociais fortes e de qualidade têm efeitos profundos sobre saúde física e mental.
- Conexão social reduz riscos de mortalidade e doenças mentais; isolamento social é fator de risco significativo.
- Apoio social de familiares e amigos diminui estresse percebido e melhora humor, reduzindo sintomas de ansiedade e depressão.
- Laços fortes — tanto presenciais quanto online — atuam como fator protetor contra depressão, enquanto conexões superficiais não geram o mesmo efeito.
- Estudos de longo prazo (Harvard Study of Adult Development) confirmam que qualidade das relações pessoais é o principal preditor de longevidade e bem‑estar.
- Fortes vínculos sociais estão ligados à manutenção da memória em idosos e ao alívio de sofrimento emocional em grupos de apoio.
- Impacto para o CheckIn: Ao facilitar encontros, conexões e participação social, a plataforma pode atuar como agente de promoção de saúde mental e bem‑estar, contribuindo para qualidade de vida e prevenção de problemas relacionados ao isolamento.

3) Síntese para justificar o CheckIn
- Avaliação + feedback = estímulo direto para melhor atendimento.
- Conexão social + engajamento = benefícios comprovados para saúde mental e física.
- O CheckIn pode unir esses dois eixos, criando uma rede de interação que ao mesmo tempo monitora qualidade de serviço e promove interações sociais positivas, resultando em impacto econômico, reputacional e de bem‑estar.


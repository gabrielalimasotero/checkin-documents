# Estratégia de Negócio

## Perguntas‑chave de avaliação e respostas do projeto

### A solução resolve um problema real com impacto mensurável?
Sim. Endereça a sobrecarga de opções e a falta de confiança ao decidir "onde ir agora" (e com quem), reduzindo atrito na descoberta e aumentando a satisfação pós‑experiência.
- Problema real: excesso de escolhas, informações dispersas e dados desatualizados; difícil conciliar preferências do grupo.
- Como resolvemos: recomendações personalizadas e contextualizadas (localização, horário, preferências, restrições), com justificativa clara e filtros essenciais.
- Impacto mensurável (exemplos de indicadores):
  - Tempo até primeira recomendação útil (time‑to‑value).
  - CTR nas top recomendações e taxa de check‑in após recomendação.
  - Retenção (D1/D7), sessões repetidas e repetição de uso em grupo.
  - Satisfação qualitativa (CSAT/NPS inicial) e feedback de utilidade das recomendações.

### Há clareza sobre como a solução cria valor para a organização ou usuário?
Sim. O valor é explícito para ambos os lados e se reforça com o uso contínuo.
- Para o usuário: decisão mais rápida e confiante; filtros objetivos (ex.: aceita cartão, cadeirinha, estacionamento); recomendações explicadas; coordenação social facilitada; experiência mais satisfatória.
- Para a organização/produto:
  - Engajamento e retenção: jornadas curtas com alto valor percebido elevam o uso recorrente.
  - Ativos de dados: check‑ins/avaliações enriquecem o ranking e melhoram recomendações futuras (efeito flywheel).
  - Parcerias/receita (futuro): reservas/afiliados, ofertas locais e posicionamento patrocinado com guardrails de relevância.
  - Eficiência operacional: stack simples (PaaS, ETL incremental) e uso de LLM restrito ao que agrega valor (intenção/rationale), controlando custo e latência.
  - Escalabilidade do valor: arquitetura preparada para novos domínios (eventos, turismo, vida noturna), ampliando casos de uso e base endereçável.

## Modelos de monetização potenciais
- Afiliados e reservas: comissão por reserva/lead gerado para estabelecimentos parceiros.
- Ofertas locais: promoções segmentadas e time‑boxed para aumentar fluxo em horários de baixa demanda.
- Posicionamento patrocinado: destaque pago para estabelecimentos, com guardrails fortes de relevância e transparência para não degradar a experiência.
- Assinatura B2B: insights e presença premium para estabelecimentos (dados agregados de demanda, preferências, feedback), respeitando privacidade.
- Assinatura B2C (opcional): benefícios ao usuário (ex.: filtros avançados, recomendações exclusivas) preservando o core gratuito.


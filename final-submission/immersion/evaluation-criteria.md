# Fase de Imersão

## Perguntas‑chave de avaliação e respostas do projeto

### O domínio escolhido é bem definido e relevante?
Sim. O domínio é a descoberta e recomendação de lugares (principalmente restaurantes e experiências sociais locais) baseada em avaliações e contexto do usuário. Está bem delimitado por:
- Escopo de negócio: curadoria e recomendação personalizada para decidir “onde ir agora”, reduzindo atrito na descoberta.
- Fronteiras técnicas: integrações com provedores de dados locais, enriquecimento com preferências do usuário e sinais comportamentais.
- Relevância: decisões de consumo de lazer/alimentação dependem fortemente de reviews e disponibilidade; alto volume de buscas locais; impacto direto em retenção e engajamento.
- Alinhamento com o CheckIn: reforça os pilares de descoberta, confiança (reviews) e utilidade em mobilidade.

### Há clareza sobre a persona principal e suas necessidades?
Sim. Persona principal (resumo):
- Perfil: “Explorador Urbano”, 20–35 anos, usa smartphone para decidir onde comer/ir com amigos, valoriza experiências, custo‑benefício e conveniência.
- Contexto: decide com pouco tempo, muitas opções, alta incerteza sobre qualidade/tempo de espera.
- Dores: excesso de opções; medo de “escolha ruim”; dificuldade em conciliar preferências do grupo; informações dispersas.
- Necessidades: recomendações confiáveis e rápidas; filtros por restrições (ex.: aceita cartão, cadeirinha, estacionamento); informações atualizadas e geolocalizadas.
- Sucesso: escolha assertiva em poucos toques; satisfação pós‑visita; vontade de retornar/indicar.

### As fontes de dados estão bem mapeadas e justificadas?
Sim. Fonte principal utilizada: TripAdvisor.

Dados coletados:
- Nome e categoria do estabelecimento
- Localização (latitude/longitude)
- Horários de funcionamento
- Avaliações e notas de usuários
- Metadados (ex.: "aceita cartão", "possui cadeirinha de criança", "estacionamento", etc.)

Justificativa:
- Dados ricos e atualizados diretamente por usuários e pelos próprios estabelecimentos
- Ampla cobertura geográfica
- Informação de alta relevância para recomendações personalizadas

Limitações reconhecidas:
- Dependência de um único provedor
- Possíveis gaps de cobertura em cidades menores

Mitigação:
- Estruturar o código de integração para permitir incluir outras fontes (ex.: Google Places, Foursquare, Yelp, OpenStreetMap/OSM) no futuro sem grande refatoração (abstração por interfaces, normalização de esquema e testes de contrato)

---


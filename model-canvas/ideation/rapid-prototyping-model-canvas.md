# Canvas de Prototipação Rápida - CheckIn

## Visão Geral
Este canvas documenta a estratégia de prototipação rápida para o CheckIn, permitindo validação ágil de conceitos, testes de usabilidade e iteração baseada em feedback real dos usuários.

---

## 1. Objetivos da Prototipação

### 1.1 Objetivos Primários
- **Validação de Conceito**: Testar se usuários compreendem e valorizam a proposta do assistente de IA
- **Teste de Fluxos**: Validar jornadas principais (Quero sair → Sugestões → Check-in)
- **Feedback de UX**: Identificar fricções na interface e navegação
- **Prova de Conceito Técnica**: Demonstrar viabilidade da integração com IA

### 1.2 Objetivos Secundários
- Engajar stakeholders e potenciais investidores
- Facilitar desenvolvimento com especificações visuais claras
- Reduzir riscos de retrabalho na implementação
- Criar material para testes com estabelecimentos parceiros

---

## 2. Estratégia de Fidelidade Progressiva

### 2.1 Fase 1: Baixa Fidelidade (Semana 1-2)
**Objetivo**: Validar conceito e fluxos principais

**Ferramentas**: 
- Figma para wireframes
- Papel e caneta para sketches iniciais
- Marvel/InVision para navegação básica

**Entregas**:
- Wireframes das 5 telas principais
- Fluxo "Quero sair" completo
- Navegação entre abas principais

**Validação**:
- 3-5 usuários das personas identificadas
- Teste de compreensão do conceito
- Feedback sobre fluxo de navegação

### 2.2 Fase 2: Média Fidelidade (Semana 3-4)
**Objetivo**: Refinar interface e validar interações

**Ferramentas**:
- Figma com componentes do design system
- Protótipo clicável com transições
- Conteúdo real (estabelecimentos locais)

**Entregas**:
- Protótipo navegável completo
- Design system básico implementado
- Simulação de sugestões da IA

**Validação**:
- 5-8 usuários com tarefas específicas
- Teste de usabilidade moderado
- Métricas: tempo de conclusão, taxa de erro

### 2.3 Fase 3: Alta Fidelidade (Semana 5-6)
**Objetivo**: Validar experiência final e preparar desenvolvimento

**Ferramentas**:
- Figma com design system completo
- Protótipo com micro-interações
- Simulação de dados reais

**Entregas**:
- Protótipo pixel-perfect
- Especificações técnicas detalhadas
- Guia de implementação para desenvolvedores

**Validação**:
- 8-10 usuários em ambiente controlado
- Teste A/B de variações de interface
- Validação com desenvolvedores

---

## 3. Ferramentas e Tecnologias

### 3.1 Design e Prototipação
| Ferramenta | Uso | Fase |
|------------|-----|------|
| **Figma** | Design principal, componentes, handoff | Todas |
| **FigJam** | Brainstorming, user journey mapping | 1-2 |
| **ProtoPie** | Micro-interações complexas | 3 |
| **Principle** | Animações e transições | 2-3 |

### 3.2 Validação e Testes
| Ferramenta | Uso | Fase |
|------------|-----|------|
| **Maze** | Testes de usabilidade não moderados | 2-3 |
| **Lookback** | Testes moderados remotos | Todas |
| **Hotjar** | Gravação de sessões e heatmaps | 3 |
| **Google Forms** | Questionários pós-teste | Todas |

### 3.3 Colaboração
| Ferramenta | Uso | Fase |
|------------|-----|------|
| **Figma Comments** | Feedback da equipe | Todas |
| **Slack** | Comunicação rápida | Todas |
| **Trello** | Tracking de tasks de design | Todas |
| **GitHub** | Documentação técnica | 2-3 |

---

## 4. Fluxos Prioritários para Prototipação

### 4.1 Fluxo Principal: "Quero Sair"
```
1. Home → Botão "Quero sair"
2. Filtros contextuais (sozinho/amigos/casal)
3. IA processa e gera sugestões
4. Lista de 3 recomendações com detalhes
5. Seleção de local → Detalhes do estabelecimento
6. Ação: Reservar, Check-in ou Favoritar
```

**Prioridade**: Crítica
**Tempo estimado**: 3 dias de design + 2 dias de prototipação

### 4.2 Fluxo Secundário: Check-in e Experiência
```
1. Check-in no local (manual ou QR)
2. Confirmação de presença
3. Acesso ao menu/pedidos (se aplicável)
4. Avaliar experiência pós-visita
5. Compartilhar nas redes (opcional)
```

**Prioridade**: Alta
**Tempo estimado**: 2 dias de design + 1 dia de prototipação

### 4.3 Fluxo Social: Convites e Eventos
```
1. Criar evento/convite
2. Selecionar amigos
3. Escolher local (sugestão ou manual)
4. Enviar convites
5. Acompanhar RSVPs
6. Gerenciar evento ativo
```

**Prioridade**: Média
**Tempo estimado**: 2 dias de design + 1 dia de prototipação

---

## 5. Componentes e Padrões de Design

### 5.1 Componentes Prioritários
| Componente | Descrição | Estados |
|------------|-----------|---------|
| **SuggestionCard** | Card de recomendação de local | Default, Loading, Selected |
| **AssistantChat** | Interface de conversa com IA | Typing, Response, Error |
| **CheckInButton** | Botão de check-in principal | Available, Processing, Success |
| **VenueCard** | Cartão de estabelecimento | Compact, Detailed, Favorited |
| **FilterTabs** | Abas de filtro contextual | Active, Inactive, Badge |

### 5.2 Micro-interações Essenciais
- **Sugestão carregando**: Animação de "pensando" da IA
- **Card selection**: Feedback visual ao selecionar local
- **Pull to refresh**: Atualizar sugestões na home
- **Success states**: Confirmação de check-in, convite enviado
- **Loading states**: Skeleton screens para conteúdo dinâmico

---

## 6. Cenários de Teste

### 6.1 Personas e Cenários
#### **Julia (Casal Frustrado)**
- **Cenário**: Sexta 19h, quer jantar romântico com namorado
- **Teste**: Fluxo "Quero sair" → Filtro "Encontro" → Sugestões românticas
- **Métricas**: Tempo até encontrar opção satisfatória, qualidade percebida

#### **Carlos (Pai Social)**
- **Cenário**: Sábado 18h, quer sair com família (criança de 6 anos)
- **Teste**: Filtro "Família" → Sugestões family-friendly → RSVP
- **Métricas**: Adequação das sugestões, facilidade de organizar grupo

#### **André (Prático)**
- **Cenário**: Quinta 18h30, voltando do trabalho, quer parar rapidinho
- **Teste**: Localização automática → Sugestões "no caminho" → Check-in rápido
- **Métricas**: Velocidade do fluxo, relevância das sugestões de rota

#### **Mariana (Discreta)**
- **Cenário**: Domingo 15h, quer café tranquilo para ler
- **Teste**: Filtro "Sozinha" → Sugestões ambientes calmos → Avaliação pós-visita
- **Métricas**: Precisão na seleção de ambientes, satisfação com a experiência

### 6.2 Tarefas de Teste Específicas
1. **Compreensão**: "Explique com suas palavras o que este app faz"
2. **Navegação**: "Encontre um local para jantar com amigos hoje à noite"
3. **Decisão**: "Compare as 3 sugestões e escolha uma"
4. **Social**: "Convide 2 amigos para ir junto"
5. **Check-in**: "Registre sua presença no local escolhido"

---

## 7. Métricas e Validação

### 7.1 Métricas Quantitativas
| Métrica | Meta | Ferramenta |
|---------|------|------------|
| **Task Success Rate** | >90% | Maze/Lookback |
| **Time on Task** | <3 min para fluxo principal | Timer manual |
| **Error Rate** | <5% | Observação direta |
| **SUS Score** | >70 | Questionário SUS |
| **Net Promoter Score** | >50 | Questionário NPS |

### 7.2 Métricas Qualitativas
- **Compreensão do conceito**: % que entende a proposta sem explicação
- **Desejo de uso**: % que baixaria o app se existisse
- **Confiança na IA**: % que confiaria nas sugestões do assistente
- **Adequação das sugestões**: Relevância percebida das recomendações
- **Facilidade de uso**: Feedback sobre intuitividade da interface

### 7.3 Critérios de Aprovação por Fase
#### **Fase 1 (Baixa Fidelidade)**
- ✅ 80% dos usuários compreendem o conceito
- ✅ Fluxo principal concluído sem ajuda
- ✅ Feedback geral positivo sobre a ideia

#### **Fase 2 (Média Fidelidade)**
- ✅ SUS Score > 60
- ✅ Task Success Rate > 85%
- ✅ Tempo médio < 4 minutos para fluxo completo

#### **Fase 3 (Alta Fidelidade)**
- ✅ SUS Score > 70
- ✅ NPS > 50
- ✅ 90% concluiriam fluxo sem erros
- ✅ Aprovação técnica para implementação

---

## 8. Cronograma de Execução

### 8.1 Sprint 1 (Semanas 1-2): Conceito e Wireframes
**Dias 1-3**: Sketches e ideação
- Brainstorming de soluções
- Crazy 8s para geração de ideias
- Definição de fluxos principais

**Dias 4-7**: Wireframes digitais
- Criação de wireframes no Figma
- Definição de arquitetura de informação
- Review com equipe técnica

**Dias 8-10**: Primeiro protótipo
- Linkagem de telas no Figma
- Primeiro teste com 3 usuários
- Iteração baseada em feedback

### 8.2 Sprint 2 (Semanas 3-4): Design e Validação
**Dias 11-14**: Design visual
- Aplicação do design system
- Criação de componentes reutilizáveis
- Definição de micro-interações

**Dias 15-17**: Protótipo interativo
- Implementação de transições
- Simulação de conteúdo real
- Estados de loading e erro

**Dias 18-20**: Testes de usabilidade
- 5-8 sessões de teste moderado
- Análise de métricas quantitativas
- Documentação de insights

### 8.3 Sprint 3 (Semanas 5-6): Refinamento e Handoff
**Dias 21-24**: Refinamento final
- Correções baseadas em testes
- Polimento visual e interações
- Validação com stakeholders

**Dias 25-27**: Preparação para desenvolvimento
- Especificações técnicas detalhadas
- Assets exportados para desenvolvimento
- Guias de implementação

**Dias 28-30**: Validação final
- Último round de testes
- Aprovação de stakeholders
- Kickoff com equipe de desenvolvimento

---

## 9. Documentação e Entregáveis

### 9.1 Para Cada Fase
**Documentos**:
- Relatório de testes com insights principais
- Versão atualizada do protótipo
- Lista de melhorias identificadas
- Roadmap de próximas iterações

**Arquivos Figma**:
- Wireframes organizados por fluxo
- Componentes do design system
- Protótipo navegável
- Especificações para desenvolvimento

### 9.2 Entrega Final
**Design System**:
- Biblioteca de componentes completa
- Tokens de design (cores, tipografia, espaçamentos)
- Guia de uso para desenvolvedores
- Estados e variações de cada componente

**Protótipo Final**:
- Versão navegável com todos os fluxos
- Simulação realista de conteúdo
- Documentação de interações
- Link para demonstrações

**Relatório de Validação**:
- Síntese de todos os testes realizados
- Métricas atingidas vs. metas
- Recomendações para implementação
- Roadmap de melhorias futuras

---

**Responsável**: Gabriela Lima Sotero (UX/UI Designer)  
**Colaboração**: Equipe técnica para viabilidade  
**Validação**: Usuários representativos das personas  
**Timeline**: 6 semanas (3 sprints de 2 semanas)  
**Última atualização**: Dezembro 2024

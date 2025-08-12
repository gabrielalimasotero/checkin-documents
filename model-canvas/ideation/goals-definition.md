# Canvas de Definição de Objetivos - CheckIn

## Instruções de Preenchimento

Este canvas traduz as metas estratégicas do CheckIn em objetivos operacionais, fornecendo orientações claras para a execução técnica e o monitoramento de resultados da plataforma social de check-in em estabelecimentos.

---

## 1. Revisão do Objetivo Geral

**Objetivo Principal Confirmado:**
Criar uma plataforma social móvel que conecta pessoas através de experiências compartilhadas em estabelecimentos, reduzindo o tempo de decisão para sair e facilitando conexões sociais autênticas baseadas em contexto e localização.

**Refinamento:**
O CheckIn vai além de ser um app de descoberta - é um assistente inteligente que responde às perguntas fundamentais: *"Para onde eu vou sair hoje?"* e *"Com quem eu vou?"*, utilizando IA conversacional e curadoria social em tempo real.

---

## 2. Refinamento de Objetivos Específicos

### 2.1 Redução do Tempo de Decisão
- Diminuir o tempo médio para escolher onde sair de 15-30 minutos para menos de 2 minutos, utilizando sugestões personalizadas por IA
- Implementar botão "Quero sair" que gera recomendações contextuais instantâneas baseadas em localização, horário e preferências

### 2.2 Facilitação de Conexões Sociais
- Aumentar a taxa de encontros sociais bem-sucedidos em 60% através do sistema de convites inteligentes e RSVP
- Conectar usuários por afinidade (trabalho, estudo, interesses) e não apenas proximidade geográfica

### 2.3 Melhoria da Experiência em Estabelecimentos
- Reduzir frustrações com "rolês ruins" em 70% através de avaliações sociais em tempo real (vibe, música, público)
- Implementar sistema de check-in que melhore a gestão de fluxo para estabelecimentos em 50%

### 2.4 Engajamento e Retenção
- Atingir taxa de retorno semanal de 80% através de sugestões precisas e experiências sociais satisfatórias
- Criar 500 check-ins únicos por mês no MVP com base de 100 usuários ativos

---

## 3. Mapeamento de Indicadores Operacionais de Sucesso

### 3.1 Métricas de Decisão
- **Tempo até decisão:** Cronometragem desde abertura do app até seleção de local
- **Taxa de conversão:** % de sugestões aceitas vs. rejeitadas
- **Ferramenta:** Analytics integrado no app + logs de eventos de usuário

### 3.2 Métricas de Engajamento Social
- **Check-ins realizados:** Número de check-ins mensais por usuário ativo
- **Convites enviados/aceitos:** Taxa de sucesso de convites sociais
- **Ferramenta:** Dashboard de métricas sociais + notificações push tracking

### 3.3 Métricas de Qualidade da Experiência
- **Rating pós-check-in:** Avaliação média das experiências
- **Taxa de check-ins repetidos:** % de usuários que voltam ao mesmo local
- **Ferramenta:** Sistema de avaliações integrado + análise de padrões comportamentais

### 3.4 Métricas de Estabelecimentos
- **Estabelecimentos ativos:** Número de locais com check-ins mensais
- **Taxa de conversão para Premium:** % de estabelecimentos que migram para plano pago
- **Ferramenta:** Dashboard de estabelecimentos + sistema de cobrança

---

## 4. Metas Quantitativas Refinadas

### 4.1 MVP (Primeiros 3 meses)
- **Usuários:** 100 usuários ativos mensais
- **Check-ins:** 500 check-ins únicos por mês
- **Estabelecimentos:** 25 estabelecimentos cadastrados (10 ativos)
- **Tempo de decisão:** Redução para menos de 3 minutos
- **Taxa de retorno:** 60% dos usuários voltam em 1 semana

### 4.2 Escala Inicial (6 meses)
- **Usuários:** 1.000 usuários ativos mensais
- **Check-ins:** 3.000 check-ins únicos por mês
- **Estabelecimentos:** 100 estabelecimentos (50 estabelecimentos premium)
- **Tempo de decisão:** Redução para menos de 2 minutos
- **Taxa de conversão social:** 40% dos convites aceitos

### 4.3 Crescimento (12 meses)
- **Usuários:** 10.000 usuários ativos mensais
- **Receita:** R$ 50k MRR através de planos premium
- **Estabelecimentos:** 500 estabelecimentos (200 premium)
- **Expansão geográfica:** 3 cidades principais
- **NPS:** Score acima de 50

---

## 5. Priorização Baseada em Impacto e Viabilidade

### 5.1 Alta Prioridade (Sprint 1-2)
**Sugestões com IA + Check-in básico**
- **Impacto:** Alto - Core value proposition do produto
- **Viabilidade:** Alta - Tecnologia disponível (OpenAI API)
- **Justificativa:** Diferencial competitivo principal, validação rápida com usuários

### 5.2 Média Prioridade (Sprint 3-4)
**Sistema social (convites/RSVP) + Avaliações**
- **Impacto:** Alto - Aumenta retenção e engajamento
- **Viabilidade:** Média - Requer desenvolvimento de features sociais complexas
- **Justificativa:** Essencial para criação de rede ativa, mas depende de massa crítica

### 5.3 Baixa Prioridade (Pós-MVP)
**Monetização + Analytics avançados**
- **Impacto:** Médio - Sustentabilidade financeira
- **Viabilidade:** Alta - Implementação direta após validação
- **Justificativa:** Importante para escala, mas prematura sem validação de mercado

---

## 6. Identificação de Ações e Recursos Necessários

### 6.1 Desenvolvimento da IA Conversacional
**Ações:**
- Integrar OpenAI GPT-4 para processamento de contexto e sugestões
- Desenvolver prompt engineering para respostas contextuais sobre locais
- Implementar sistema de aprendizado baseado em feedback de usuários

**Recursos:**
- 1 Desenvolvedor Full-stack especializado em IA (Henrique)
- Orçamento para APIs OpenAI: $200/mês inicialmente
- Base de dados de estabelecimentos e tags (crowd-sourced + parceiros)

### 6.2 Desenvolvimento do Sistema Social
**Ações:**
- Implementar sistema de autenticação JWT e perfis de usuário
- Desenvolver funcionalidades de convites, RSVP e check-ins
- Criar sistema de notificações push e avaliações sociais

**Recursos:**
- 2 Desenvolvedores Frontend React/TypeScript (João Pedro, Lucas Luis)
- 1 Desenvolvedor Backend FastAPI (João Victor)
- Ferramentas: Supabase para auth, push notifications service

### 6.3 Design e UX
**Ações:**
- Finalizar wireframes e protótipos navegáveis
- Implementar design system com Shadcn/ui e Tailwind
- Conduzir testes de usabilidade com 6-10 usuários por sprint

**Recursos:**
- 1 Designer UX/UI (Gabriela - PO)
- Ferramentas: Figma Pro, ferramentas de prototipação
- Budget para testes de usuário: R$ 2.000

### 6.4 Infraestrutura e Deploy
**Ações:**
- Configurar ambiente de desenvolvimento e produção
- Implementar CI/CD pipeline multi-repositório
- Configurar monitoramento e analytics

**Recursos:**
- 1 Especialista em Infraestrutura (Lucas Emmanuel)
- Serviços cloud: Vercel (frontend), Railway/Heroku (backend)
- PostgreSQL managed database
- Orçamento inicial: R$ 500/mês para infraestrutura

### 6.5 Aquisição de Estabelecimentos
**Ações:**
- Desenvolver pitch deck para estabelecimentos
- Criar onboarding simplificado (5 minutos)
- Implementar programa piloto com 10 estabelecimentos parceiros

**Recursos:**
- Time comercial: Gabriela (PO) + suporte da equipe
- Material marketing: landing pages, apresentações
- Incentivos para estabelecimentos piloto: 3 meses premium gratuito

---

## 7. Cronograma de Validação

### Semana 1-2: Validação de Conceito
- Protótipo navegável com sugestões mockadas
- Testes com 5 usuários das personas identificadas
- Métricas: tempo de compreensão do conceito, interesse declarado

### Semana 3-4: Validação de IA
- MVP com IA real integrada
- Teste de qualidade das sugestões com 10 usuários
- Métricas: relevância das sugestões, satisfação com respostas

### Semana 5-8: Validação Social
- Recursos sociais básicos implementados
- Teste com grupos de 3-5 pessoas conhecidas
- Métricas: uso de convites, taxa de check-ins em grupo

### Semana 9-12: Validação de Mercado
- MVP completo com estabelecimentos reais
- Teste com 50 usuários e 10 estabelecimentos
- Métricas: retenção, NPS, feedback qualitativo, viabilidade comercial

---

**Responsável pela execução:** Equipe CheckIn  
**Revisão:** Quinzenal nas retrospectivas de sprint  
**Aprovação:** PO (Gabriela Lima Sotero)  
**Data de criação:** Dezembro 2024

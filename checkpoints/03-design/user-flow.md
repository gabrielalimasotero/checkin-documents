# User Flow - CheckIn App

## 📱 Visão Geral do App

O CheckIn é um aplicativo mobile que permite aos usuários fazer check-ins em estabelecimentos, gerenciar contas abertas, explorar lugares e pessoas, e manter uma rede social baseada em localização.

## 🏗️ Estrutura de Navegação

### **Navegação Principal**
```
┌─────────────────────────────────────┐
│ Home | Check In | [+] | Social | Profile │
└─────────────────────────────────────┘
```

### **Páginas Principais**
- **Home** - Feed principal com 3 abas
- **Check In** - Gerenciamento de check-ins e contas
- **Social** - Exploração de pessoas e lugares
- **Profile** - Perfil do usuário
- **Botão [+]** - Check-in rápido

## 🏠 Home Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ [Logo] CheckIn [🔍] [💬] [🔔]       │
├─────────────────────────────────────┤
│ [Explore] [Network] [For You]       │
├─────────────────────────────────────┤
│ Conteúdo da aba selecionada         │
└─────────────────────────────────────┘
```

### **Aba Explore**
- **Funcionalidade**: Descoberta de lugares e eventos
- **Conteúdo**: Cards de estabelecimentos, eventos, atividades
- **Ações**: Ver detalhes, reservar, favoritar

### **Aba Network**
- **Funcionalidade**: Rede social e conexões
- **Conteúdo**: Posts de amigos, check-ins, atividades
- **Ações**: Curtir, comentar, compartilhar

### **Aba For You**
- **Funcionalidade**: Sugestões personalizadas
- **Conteúdo**: Feed de sugestões + botão "Quero sair"
- **Fluxo "Quero sair"**:
  ```
  [Quero sair] → Filtros → Seleção/Exploração
  ```

#### **Fluxo "Quero sair" Detalhado**
```
1. [Quero sair] (1 toque)
   ↓
2. Filtros:
   ├── 🚶 Sozinho → Feed filtrado
   ├── 👥 Com amigos → Selecionar/Conhecer
   ├── 💕 Encontro → Selecionar/Conhecer
   └── 👨‍👩‍👧‍👦 Grupos → Selecionar/Conhecer
   ↓
3. Opções (para amigos/encontro/grupos):
   ├── [Selecionar] → Lista de contatos
   └── [Conhecer] → Tela de exploração
```

## 📍 Check In Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ Check-in                            │
│ Gerencie suas experiências    [+]   │
├─────────────────────────────────────┤
│ [Geral] [Histórico] [Ativo]         │
├─────────────────────────────────────┤
│ Conteúdo da aba selecionada         │
└─────────────────────────────────────┘
```

### **Aba Geral**
- **Status Atual**: Card de destaque com check-in ativo
- **Contas Abertas**: Amigos com contas não pagas
- **Finalizados Hoje**: Amigos que concluíram check-ins
- **Explorar Pessoas**: Botões para conhecer pessoas

### **Aba Histórico**
- **Histórico de Check-ins**: Lista de check-ins anteriores
- **Avaliações**: Sistema de estrelas para estabelecimentos
- **Ações**: Ver detalhes, avaliar, recheck-in

### **Aba Ativo**
- **Check-in Ativo**: Status atual do usuário
- **Minha Conta**: Itens pedidos e status
- **Pessoas Aqui**: Quem está no local
- **Pessoas que Saíram**: Histórico de presença
- **Vão Vir**: RSVPs confirmados

## 👥 Social Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ [Logo] CheckIn [🔍] [💬] [🔔]       │
├─────────────────────────────────────┤
│ [Pessoas] [Lugares] [Eventos]       │
├─────────────────────────────────────┤
│ Conteúdo da aba selecionada         │
└─────────────────────────────────────┘
```

### **Aba Pessoas**
- **Exploração**: Descobrir pessoas próximas
- **Filtros**: Por interesse, localização, disponibilidade
- **Ações**: Conectar, enviar mensagem, ver perfil

### **Aba Lugares**
- **Estabelecimentos**: Lista de lugares próximos
- **Filtros**: Por tipo, avaliação, distância
- **Ações**: Ver detalhes, fazer check-in, favoritar

### **Aba Eventos**
- **Eventos**: Shows, festas, encontros
- **Filtros**: Por data, tipo, localização
- **Ações**: RSVP, ver detalhes, compartilhar

## 👤 Profile Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ [Avatar] Nome do Usuário            │
│ @username                           │
├─────────────────────────────────────┤
│ [Posts] [Check-ins] [Favoritos]     │
├─────────────────────────────────────┤
│ Conteúdo da aba selecionada         │
└─────────────────────────────────────┘
```

### **Funcionalidades**
- **Informações Pessoais**: Bio, interesses, localização
- **Histórico**: Posts, check-ins, avaliações
- **Configurações**: Privacidade, notificações, conta

## 🔔 Notifications Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ Notificações                        │
├─────────────────────────────────────┤
│ Lista de notificações               │
│ ├── Check-ins de amigos             │
│ ├── Convites para eventos           │
│ ├── Novas conexões                  │
│ └── Atualizações de lugares         │
└─────────────────────────────────────┘
```

## 🏪 Venue Details Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ [Imagem do lugar]                   │
│ Nome do Estabelecimento             │
│ ⭐⭐⭐⭐⭐ (4.8) • 1.2km              │
├─────────────────────────────────────┤
│ [Sobre] [Menu] [Fotos] [Avaliações] │
├─────────────────────────────────────┤
│ [Fazer Check-in] [Reservar]         │
└─────────────────────────────────────┘
```

## 🔄 Fluxos Principais

### **1. Fluxo de Check-in**
```
1. Usuário chega no local
2. Abre o app → Check In
3. [Fazer Check-in] ou Botão [+]
4. Confirma localização
5. Insere código da mesa (opcional)
6. Check-in realizado
7. Acesso ao menu e conta
```

### **2. Fluxo de Pedido**
```
1. Check-in ativo
2. Aba Ativo → Minha Conta
3. [Adicionar itens]
4. Seleciona do menu
5. Confirma pedido
6. Acompanha status
7. Paga conta
```

### **3. Fluxo de Exploração**
```
1. Home → For You
2. [Quero sair]
3. Escolhe filtro (sozinho/amigos/encontro/grupos)
4. Seleciona ou explora
5. Escolhe lugar/atividade
6. Reserva ou faz check-in
```

### **4. Fluxo de Conexão Social**
```
1. Social → Pessoas
2. Explora pessoas próximas
3. Vê perfil de interesse
4. [Conectar]
5. Envia mensagem (opcional)
6. Marca encontro
```

## 🎯 Pontos de Entrada

### **Principais**
- **Home**: Feed principal e descoberta
- **Check In**: Gerenciamento de experiências
- **Social**: Exploração e conexões
- **Profile**: Perfil pessoal

### **Secundários**
- **Botão [+]**: Check-in rápido
- **Notificações**: Atualizações e convites
- **Venue Details**: Detalhes de estabelecimentos

## 📱 Padrões de Interação

### **Navegação**
- **Bottom Navigation**: 5 itens principais
- **Tabs**: Sub-navegação dentro das páginas
- **Back Button**: Navegação hierárquica
- **Swipe**: Navegação entre abas

### **Ações**
- **Tap**: Seleção e navegação
- **Long Press**: Ações contextuais
- **Swipe**: Ações rápidas (favoritar, excluir)
- **Pull to Refresh**: Atualizar conteúdo

### **Feedback**
- **Loading States**: Indicadores de carregamento
- **Success/Error**: Toasts e alertas
- **Haptic Feedback**: Vibração para ações importantes
- **Visual Feedback**: Animações e transições

## 🔒 Estados do Usuário

### **Não Autenticado**
- Acesso limitado a funcionalidades básicas
- Redirecionamento para login/registro

### **Autenticado**
- Acesso completo a todas as funcionalidades
- Personalização baseada em histórico

### **Check-in Ativo**
- Acesso ao menu e pedidos
- Visibilidade para amigos
- Funcionalidades de pagamento

## 📊 Métricas de Engajamento

### **Principais**
- **Check-ins realizados**
- **Tempo de permanência**
- **Interações sociais**
- **Reservas e pedidos**

### **Secundárias**
- **Exploração de lugares**
- **Conexões feitas**
- **Avaliações deixadas**
- **Compartilhamentos**

## 🚀 Próximos Passos

### **Funcionalidades Planejadas**
- **Chat em tempo real**
- **Pagamentos integrados**
- **Recomendações avançadas**
- **Gamificação**

### **Melhorias de UX**
- **Onboarding otimizado**
- **Acessibilidade aprimorada**
- **Performance mobile**
- **Offline mode**

---

**Versão**: 1.0  
**Última atualização**: Dezembro 2024  
**Status**: Implementado

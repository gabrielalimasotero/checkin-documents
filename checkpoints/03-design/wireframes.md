# Wireframes - CheckIn App

## 📱 Visão Geral

Este documento apresenta os wireframes das principais telas do aplicativo CheckIn, mostrando a estrutura visual e layout dos componentes implementados.

Nota: A prototipação desta fase foi feita majoritariamente no Lovable (sem uso de Figma).

## 🏗️ Estrutura Base

### **Container Mobile**
```
┌─────────────────────────────────────┐
│ Safe Area Top (20px)                │
├─────────────────────────────────────┤
│ Header/Navigation                   │
├─────────────────────────────────────┤
│ Content Area                        │
│ (Scrollable)                        │
├─────────────────────────────────────┤
│ Bottom Navigation                   │
├─────────────────────────────────────┤
│ Safe Area Bottom (20px)             │
└─────────────────────────────────────┘
```

**Especificações**:
- **Largura**: 390px (iPhone 14 Pro)
- **Altura**: 844px (iPhone 14 Pro)
- **Safe Areas**: 20px top/bottom
- **Padding**: 16px horizontal

## 🔐 Login Page

### **Estrutura**
```
┌─────────────────────────────────────┐
│ CheckIn                              │
├─────────────────────────────────────┤
│ [Email]                              │
│ [Senha]                              │
│ [Entrar] [Criar conta]               │
│ [Entrar com Google/Apple] (opcional) │
└─────────────────────────────────────┘
```

## 🏠 Home Page

### **Header**
```
┌─────────────────────────────────────┐
│ CheckIn        [🔍] [💬] [🔔]      │
└─────────────────────────────────────┘
```

### **Sub Navigation**
```
┌─────────────────────────────────────┐
│ [Network] [For You] [Destaques]     |
│ [Convites] [Mensagens] [Notificações] |
└─────────────────────────────────────┘
```

### **For You Tab - Tela Principal**
```
┌─────────────────────────────────────┐
│ [Quero sair]                        │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Imagem]                        │ │
│ │ Bistrô do Chef                  │ │
│ │ Francesa                        │ │
│ │ Perfeito para um jantar...      │ │
│ │ ⭐⭐⭐⭐⭐ 4.8 • 1.2km           │ │
│ │ 💡 Baseado no seu gosto...      │ │
│ │ [Ver detalhes] [Reservar]       │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Imagem]                        │ │
│ │ Show Acústico - João Martins    │ │
│ │ Café Cultural                   │ │
│ │ Música brasileira ao vivo...    │ │
│ │ ⭐⭐⭐⭐⭐ 4.6 • 0.8km           │ │
│ │ 💡 Você gosta de música...      │ │
│ │ [Ver detalhes] [Reservar]       │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **For You Tab - Filtros "Quero sair"**
```
┌─────────────────────────────────────┐
│ ← Quero sair                        │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 🚶 Sozinho                      │ │
│ │ Atividades para fazer sozinho   │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 👥 Com amigos                   │ │
│ │ Selecionar amigos ou conhecer   │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 💕 Encontro                     │ │
│ │ Selecionar pessoa ou conhecer   │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 👨‍👩‍👧‍👦 Grupos                 │ │
│ │ Selecionar grupo ou conhecer    │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **For You Tab - Tela de Amigos**
```
┌─────────────────────────────────────┐
│ ← Com amigos                        │
├─────────────────────────────────────┤
│ ┌─────────────┐ ┌─────────────────┐ │
│ │ [✓]         │ │ [+]             │ │
│ │ Selecionar  │ │ Conhecer        │ │
│ └─────────────┘ └─────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Ana Costa              │ │
│ │ 🟢 Online                       │ │
│ │                    [Convidar]   │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] João Santos            │ │
│ │ 🟢 Online                       │ │
│ │                    [Convidar]   │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Network Tab**
```
┌─────────────────────────────────────┐
│ ← Network                           │
├─────────────────────────────────────┤
│ [Stories/Status de amigos]          │
├─────────────────────────────────────┤
│ [Feed de atividade de amigos]       │
│ • Check-ins, convites, avaliações   │
│ • Ações: Curtir, Comentar, Convidar │
└─────────────────────────────────────┘
```

### **Destaques Tab**
```
┌─────────────────────────────────────┐
│ [Todos] [Promoções] [Restaurantes]  │
│ [Bares] [Shows] [Eventos]           │
├─────────────────────────────────────┤
│ Cards com curadoria por categoria   │
│ • Promoções ativas                  │
│ • Lugares em alta                   │
│ • Eventos do dia                    │
└─────────────────────────────────────┘
```

### **Convites Tab**
```
┌─────────────────────────────────────┐
│ Convites recebidos                  │
├─────────────────────────────────────┤
│ [Avatar] João • Hoje 20h            │
│ Bar do Centro • 1.2km               │
│ [Aceitar] [Recusar] [Ver detalhes]  │
├─────────────────────────────────────┤
│ ...                                 │
└─────────────────────────────────────┘
```

### **Mensagens Tab**
```
┌─────────────────────────────────────┐
│ Conversas                           │
├─────────────────────────────────────┤
│ [Avatar] Ana • "Bora hoje?" 2m     │
│ [Avatar] Grupo Amigos • 3 novas     │
│ ...                                 │
└─────────────────────────────────────┘
```

### **Notificações Tab**
```
┌─────────────────────────────────────┐
│ Notificações                        │
├─────────────────────────────────────┤
│ Check-ins de amigos                 │
│ Convites para eventos               │
│ Novas conexões                      │
│ Atualizações de lugares             │
└─────────────────────────────────────┘
```

## 📍 Check In Page

### **Header**
```
┌─────────────────────────────────────┐
│ Check-in                            │
│ Gerencie suas experiências    [+]   │
└─────────────────────────────────────┘
```

### **Sub Navigation**
```
┌─────────────────────────────────────┐
│ [Geral] [Ativo] [Histórico]         │
└─────────────────────────────────────┘
```

### **Aba Geral - Status Atual**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ 🎯 Status Atual          [Ativo] │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ 🏠 Boteco da Maria          │ │ │
│ │ │ Mesa 8 • Desde 18:00    [👁]│ │ │
│ │ └─────────────────────────────┘ │ │
│ │ [Ocultar check-in]              │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 👥 Contas Abertas        [2]    │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Avatar] Ana Costa          │ │ │
│ │ │ Bar do João                 │ │ │
│ │ │ há 30 min              [Ver]│ │ │
│ │ └─────────────────────────────┘ │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 🔍 Explorar Pessoas             │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ 👥 Conhecer pessoas novas   │ │ │
│ │ │ Descubra conexões próximas  │ │ │
│ │ └─────────────────────────────┘ │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Aba Histórico**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ 📚 Histórico de Check-ins  [2]  │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Imagem] Café Central       │ │ │
│ │ │ ⏰ Anteontem 14:20          │ │ │
│ │ │ [Finalizado] R$ 32.00       │ │ │
│ │ │ ⭐⭐⭐⭐⭐                    │ │ │
│ │ │                    [Ver]    │ │ │
│ │ └─────────────────────────────┘ │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Aba Ativo - Check-in Ativo**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ 📍 Check-in Ativo        [Ativo] │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Imagem] Boteco da Maria    │ │ │
│ │ │ ⏰ Hoje 18:00               │ │ │
│ │ │                    [Ver]    │ │ │
│ │ └─────────────────────────────┘ │ │
│ │ [Ocultar check-in]              │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 💳 Minha Conta            [3]   │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Icon] Chopp Brahma 300ml   │ │ │
│ │ │ Qtd: 2 • há 5 min           │ │ │
│ │ │ Entregue                    │ │ │
│ │ │                    R$ 17.00 │ │ │
│ │ └─────────────────────────────┘ │ │
│ │ Total: R$ 63.00                 │ │
│ │ [Adicionar] [Pagar]             │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Aba Ativo - Pessoas**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ 👥 Pessoas Aqui            [4]   │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Avatar] Maria Silva        │ │ │
│ │ │ No local                    │ │ │
│ │ │                    [Local]  │ │ │
│ │ └─────────────────────────────┘ │ │
│ │ ┌─────────────────────────────┐ │ │
│ │ │ [Avatar] João Santos        │ │ │
│ │ │ Na mesa                     │ │ │
│ │ │                    [Mesa]   │ │ │
│ │ └─────────────────────────────┘ │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

## 🔎 Explorar Page

### **Header**
```
┌─────────────────────────────────────┐
│ CheckIn        [🔍] [💬] [🔔]      │
└─────────────────────────────────────┘
```

### **Sub Navigation**
```
┌─────────────────────────────────────┐
│ [Lugares] [Grupos] [Pessoas]        │
└─────────────────────────────────────┘
```

### **Aba Lugares**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ [Imagem] Nome do Lugar          │ │
│ │ ⭐⭐⭐⭐⭐ 4.8 • 1.2km           │ │
│ │ Tipo: Restaurante               │ │
│ │ [Ver detalhes] [Check-in]       │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Imagem] Outro Lugar            │ │
│ │ ⭐⭐⭐⭐ 4.2 • 0.5km             │ │
│ │ Tipo: Bar                       │ │
│ │ [Ver detalhes] [Check-in]       │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Aba Grupos**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Grupo: Food Lovers     │ │
│ │ 154 membros • 2 eventos ativos  │ │
│ │ Interesses: Gastronomia         │ │
│ │ [Ver grupo] [Participar]        │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Grupo: Night Out       │ │
│ │ 98 membros • 1 evento hoje      │ │
│ │ Interesses: Bares, Shows        │ │
│ │ [Ver grupo] [Participar]        │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

### **Aba Pessoas**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Nome da Pessoa         │ │
│ │ Interesses: Música, Esportes    │ │
│ │ 1.2km de distância              │ │
│ │ [Conectar] [Ver perfil]         │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Outra Pessoa           │ │
│ │ Interesses: Gastronomia, Arte   │ │
│ │ 0.8km de distância              │ │
│ │ [Conectar] [Ver perfil]         │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

## 👤 Profile Page

### **Header**
```
┌─────────────────────────────────────┐
│ [Avatar] Nome do Usuário            │
│ @username                           │
└─────────────────────────────────────┘
```

### **Sub Navigation**
```
┌─────────────────────────────────────┐
│ [Posts] [Check-ins] [Favoritos] [Configurações] │
└─────────────────────────────────────┘
```

### **Aba Posts**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Nome do Usuário        │ │
│ │ há 2h                           │ │
│ │ [Imagem do post]                │ │
│ │ Texto do post...                │ │
│ │ [❤️ 12] [💬 3] [📤]            │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ [Avatar] Nome do Usuário        │ │
│ │ há 1 dia                        │ │
│ │ [Imagem do post]                │ │
│ │ Outro texto do post...          │ │
│ │ [❤️ 8] [💬 1] [📤]             │ │
│ └─────────────────────────────────┘ │
└─────────────────────────────────────┘
```

## 🔔 Notifications Page

### Observação
As notificações também aparecem como aba dentro da Home ("Notificações").

## 🏪 Venue Details Page

### **Header com Imagem**
```
┌─────────────────────────────────────┐
│ [Imagem do estabelecimento]         │
│ Nome do Estabelecimento             │
│ ⭐⭐⭐⭐⭐ 4.8 • 1.2km              │
└─────────────────────────────────────┘
```

### **Sub Navigation**
```
┌─────────────────────────────────────┐
│ [Sobre] [Menu] [Fotos] [Avaliações] │
└─────────────────────────────────────┘
```

### **Aba Sobre**
```
┌─────────────────────────────────────┐
│ ┌─────────────────────────────────┐ │
│ │ 📍 Endereço                     │ │
│ │ Rua das Flores, 123             │ │
│ │ São Paulo, SP                   │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ ⏰ Horário de Funcionamento     │ │
│ │ Seg-Sex: 18h-00h               │ │
│ │ Sáb-Dom: 17h-01h               │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ ┌─────────────────────────────────┐ │
│ │ 📞 Contato                      │ │
│ │ (11) 99999-9999                 │ │
│ │ contato@estabelecimento.com     │ │
│ └─────────────────────────────────┘ │
├─────────────────────────────────────┤
│ [Fazer Check-in] [Reservar]         │
└─────────────────────────────────────┘
```

## 🔄 Bottom Navigation

### **Navegação Principal**
```
┌─────────────────────────────────────┐
│ [🏠] [📍] [+] [🔎] [👤]            │
│ Home  Check  Plus Explorar Profile  │
└─────────────────────────────────────┘
```

**Especificações**:
- **Altura**: 56px
- **Ícones**: 24px
- **Texto**: 12px
- **Espaçamento**: 8px entre itens

## 🎨 Componentes Base

### **Card Padrão**
```
┌─────────────────────────────────────┐
│ [Conteúdo do card]                  │
│                                     │
│ [Ações]                             │
└─────────────────────────────────────┘
```

**Especificações**:
- **Padding**: 16px
- **Border Radius**: 12px
- **Shadow**: Subtle elevation
- **Margin**: 8px entre cards

### **Button Padrão**
```
┌─────────────────────────────────────┐
│ [Texto do botão]                    │
└─────────────────────────────────────┘
```

**Especificações**:
- **Altura**: 40px (padrão), 48px (grande)
- **Padding**: 16px horizontal
- **Border Radius**: 8px
- **Font**: 14px, medium

### **Badge**
```
┌─────────────────────────────────────┐
│ [Texto do badge]                    │
└─────────────────────────────────────┘
```

**Especificações**:
- **Padding**: 4px 8px
- **Border Radius**: 12px
- **Font**: 12px
- **Altura**: 20px

## 📱 Responsividade

### **Breakpoints**
- **Mobile**: 390px (iPhone 14 Pro)
- **Tablet**: 768px (iPad)
- **Desktop**: 1024px (Desktop)

### **Adaptações**
- **Mobile**: Layout vertical, navegação bottom
- **Tablet**: Layout híbrido, navegação lateral
- **Desktop**: Layout horizontal, navegação top

## 🎯 Estados Visuais

### **Loading State**
```
┌─────────────────────────────────────┐
│ [Skeleton]                          │
│ ████████████████████████████████████ │
│ ████████████████████████████████████ │
│ ████████████████████████████████████ │
└─────────────────────────────────────┘
```

### **Empty State**
```
┌─────────────────────────────────────┐
│ [Icon]                              │
│ Nenhum item encontrado              │
│ Descrição do estado vazio           │
│ [Ação principal]                    │
└─────────────────────────────────────┘
```

### **Error State**
```
┌─────────────────────────────────────┐
│ [Icon]                              │
│ Ops! Algo deu errado                │
│ Descrição do erro                   │
│ [Tentar novamente]                  │
└─────────────────────────────────────┘
```

## 🎨 Design Tokens

### **Cores**
- **Primary**: #0F766E (Turquesa CheckIn)
- **Secondary**: #F1F5F9 (Cinza claro)
- **Background**: #FFFFFF (Branco)
- **Text**: #0F172A (Preto)
- **Muted**: #64748B (Cinza)

### **Tipografia**
- **H1**: 24px, bold
- **H2**: 20px, semibold
- **H3**: 18px, semibold
- **Body**: 16px, normal
- **Caption**: 14px, normal
- **Small**: 12px, normal

### **Espaçamento**
- **XS**: 4px
- **SM**: 8px
- **MD**: 16px
- **LG**: 24px
- **XL**: 32px

---

**Versão**: 1.0  
**Última atualização**: Dezembro 2024  
**Status**: Implementado

## 🤖 Assistente Virtual

### **Overlay/Modal Conversacional**
```
┌─────────────────────────────────────┐
│ Assistente CheckIn                  │
├─────────────────────────────────────┤
│ "Oi! Qual a vibe e com quem vai?"  │
│ [Input de chat] [Enviar]            │
├─────────────────────────────────────┤
│ [Sugestões em cards com explicação] │
│ [Salvar] [Rejeitar] [Ver detalhes]  │
└─────────────────────────────────────┘
```

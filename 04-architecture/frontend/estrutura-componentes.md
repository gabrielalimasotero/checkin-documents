
# 🧱 Estrutura de Componentes – Frontend (React Native)

A aplicação CheckIN é desenvolvida em **React Native com Expo**, com estrutura modular e separação clara entre camadas de visual, lógica e dados.

---

## 📁 Estrutura Geral de Pastas

```
checkin-front/
├── assets/              # Imagens, ícones, fontes
├── components/          # Componentes reutilizáveis (botões, cards, tags)
├── screens/             # Telas completas (Home, CheckIn, Explore, Profile)
├── navigation/          # Stack e bottom tab navigator
├── services/            # API, autenticação, IA e geolocalização
├── contexts/            # Contextos globais (auth, user, IA)
├── utils/               # Funções utilitárias (formato de data, validação)
├── constants/           # Paleta de cores, textos fixos, rotas
└── App.tsx              # Entry point com configuração inicial
```

---

## 📦 Componentes Reutilizáveis

| Componente        | Uso principal                                           |
|-------------------|---------------------------------------------------------|
| `CardEvento`      | Exibe convites e eventos com RSVP                      |
| `CardPromocao`    | Exibe promoções com imagem e tags                      |
| `Tag`             | Representa vibe, categoria, interesse                  |
| `AvatarUsuario`   | Foto ou inicial de usuários                            |
| `BotaoPadrao`     | Botões primários e secundários                         |
| `ModalConfirmacao`| Confirmação de check-in, saída ou convite              |
| `BalaoMensagem`   | Ícones de comentário, curtir e notificações            |

---

## 🔁 Navegação

O app usa **React Navigation** com tabs e stacks:

- `BottomTabNavigator` com as rotas:
  - Feed (`HomeScreen`)
  - Check-in (`CheckInScreen`)
  - Explorar (`ExploreScreen`)
  - Perfil (`ProfileScreen`)

Cada rota possui stacks internas para detalhes (ex: evento, grupo, perfil de outro usuário).

---

## ☁️ Integrações

- **API Backend (FastAPI):**
  - Auth (login, cadastro, tokens)
  - Eventos e convites
  - Locais e check-ins
  - Avaliações e notificações

- **Serviços de IA (internos ou externos):**
  - Sugestões com base em contexto
  - Geração de tags
  - Reordenação de feed

- **Geolocalização:**
  - Consulta da posição atual
  - Cálculo de raio de interesse
  - Visualização de amigos e locais no mapa

---

## 📐 Estilo e Responsividade

- Usa **Tailwind-like** com styled-components ou framework interno
- Adaptação automática para diferentes tamanhos de tela
- Suporte ao modo escuro e claro com base na paleta azul (PMS)

---

## 🧪 Testes e Validação

- Testes manuais com Expo Go
- Captura de comportamento com eventos (cliques, tempo de decisão, navegação)
- Feedback de usuários registrado para priorização

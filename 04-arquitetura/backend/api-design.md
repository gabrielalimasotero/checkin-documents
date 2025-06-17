
# 🌐 API Design – Backend (FastAPI)

A API do CheckIN foi desenvolvida com FastAPI e segue o padrão RESTful. Ela é responsável por autenticação, gerenciamento de usuários, eventos, check-ins, locais, convites e integração com sugestões inteligentes.

---

## 🔐 Autenticação

### `POST /auth/register`
- Criação de conta
- Body: nome, email, senha, etc.

### `POST /auth/login`
- Geração de token JWT
- Body: email, senha

---

## 👤 Usuários

### `GET /users/me`
- Retorna dados do usuário autenticado

### `PUT /users/me`
- Atualiza perfil do usuário

### `GET /users/search?q=...`
- Busca por nome ou interesse

---

## 🗺️ Locais

### `GET /places/`
- Lista locais disponíveis

### `GET /places/{id}`
- Detalhes de um local

### `POST /places/`
- Cadastra novo local (restaurante)

### `PATCH /places/{id}`
- Atualiza informações do local

---

## 📍 Check-in

### `POST /checkin/`
- Realiza check-in em um local

### `GET /checkin/me`
- Retorna check-ins ativos e anteriores

### `DELETE /checkin/{id}`
- Finaliza ou cancela um check-in

---

## 📅 Eventos

### `POST /events/`
- Cria novo evento

### `GET /events/nearby`
- Lista eventos próximos do usuário

### `GET /events/{id}`
- Detalhes do evento

### `POST /events/{id}/rsvp`
- Confirma ou recusa participação

---

## 🧠 Sugestões com IA

### `GET /suggestions/feed`
- Sugestões baseadas em localização, histórico e rede

### `GET /suggestions/event`
- Sugestão de local ideal para evento criado

---

## 📣 Convites e Rede

### `POST /invites/`
- Envia convite para outro usuário

### `GET /invites/received`
- Lista convites recebidos

### `POST /invites/{id}/respond`
- Aceita ou recusa convite

### `GET /friends/list`
- Lista amigos e status (presente, indo, offline)

---

## 📝 Avaliações

### `POST /ratings/`
- Avaliação de local pós check-in

### `GET /ratings/place/{id}`
- Avaliações recentes de um local

---

## ⚙️ Observações Técnicas

- Toda requisição protegida exige header `Authorization: Bearer <token>`
- A documentação interativa está disponível em `/docs` (Swagger) e `/redoc`
- Versionamento e migrações de banco via Alembic
- Estrutura modular baseada em `routers`, `models`, `schemas` e `services`


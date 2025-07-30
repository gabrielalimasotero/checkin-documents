
# 🗃️ Modelo Entidade-Relacionamento (ER) – Backend

O modelo de dados do CheckIN foi projetado para refletir as interações sociais e a dinâmica de descoberta de lugares com base em contexto. A seguir está o mapeamento das principais entidades, seus atributos e relacionamentos.

---

## 🔑 Entidades Principais

### 🧑 Usuario
- `id`: UUID (PK)
- `nome`: string
- `email`: string (único)
- `senha_hash`: string
- `foto`: URL (opcional)
- `preferencias`: JSON (ex: vibe, categorias, restrições)

**Relacionamentos:**
- 1:N com `Checkin`, `Evento`, `Convite`, `Avaliacao`

---

### 📍 Local
- `id`: UUID (PK)
- `nome`: string
- `endereco`: string
- `latitude`: float
- `longitude`: float
- `tags`: JSON (ex: “bar”, “ao ar livre”)
- `capacidade_total`: int

**Relacionamentos:**
- 1:N com `Checkin`, `Evento`, `Avaliacao`

---

### 📅 Evento
- `id`: UUID (PK)
- `criador_id`: FK → Usuario
- `local_id`: FK → Local
- `nome`: string
- `descricao`: string
- `data_hora`: datetime
- `publico`: boolean

**Relacionamentos:**
- 1:N com `Convite`
- N:1 com `Usuario`, `Local`

---

### 📍 Checkin
- `id`: UUID (PK)
- `usuario_id`: FK → Usuario
- `local_id`: FK → Local
- `data_hora_entrada`: datetime
- `data_hora_saida`: datetime (opcional)

---

### 📨 Convite
- `id`: UUID (PK)
- `evento_id`: FK → Evento
- `remetente_id`: FK → Usuario
- `destinatario_id`: FK → Usuario
- `status`: enum (`pendente`, `aceito`, `recusado`)

---

### 🌟 Avaliacao
- `id`: UUID (PK)
- `usuario_id`: FK → Usuario
- `local_id`: FK → Local
- `nota`: int (1 a 5)
- `comentario`: string (opcional)
- `tags`: JSON (ex: “cheio”, “música boa”, “fila grande”)

---

### 🤝 Amizade
- `id`: UUID (PK)
- `usuario_1_id`: FK → Usuario
- `usuario_2_id`: FK → Usuario
- `status`: enum (`pendente`, `aceita`, `bloqueada`)

---

## 🧠 Considerações

- Todas as tabelas usam UUID como chave primária
- Os relacionamentos são projetados para refletir ações sociais reais: check-in, evento, convite, amizade
- A entidade `Usuario` também contém informações de privacidade e notificações (não detalhadas aqui)
- O modelo é compatível com migrações Alembic (SQLAlchemy)

---

## 📎 Diagrama Visual

O diagrama completo em formato visual está disponível em:  
`/04-arquitetura/backend/modelo-er.png`
ou editável no [dbdiagram.io](https://dbdiagram.io) ou Draw.io


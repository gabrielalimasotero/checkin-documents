
# 🏗️ Infraestrutura – Backend CheckIN

Este documento descreve a infraestrutura atual e planejada para o backend do CheckIN, incluindo stack tecnológica, deploy, banco de dados e segurança.

---

## 🌐 Stack de Execução

- **Linguagem:** Python 3.10+
- **Framework:** FastAPI
- **Servidor:** Uvicorn (ASGI)
- **ORM:** SQLAlchemy + Alembic
- **Banco de dados:** PostgreSQL
- **Autenticação:** JWT com python-jose
- **Hospedagem (em desenvolvimento):** Render ou Railway

---

## 🧱 Organização do Código

```
checkin-back/
├── app/
│   ├── main.py               # Ponto de entrada
│   ├── routers/              # Arquivos de rota por domínio (users, auth, places)
│   ├── models/               # Definição das tabelas SQLAlchemy
│   ├── schemas/              # Pydantic para entrada/saída
│   ├── services/             # Regras de negócio
│   ├── database.py           # Conexão e sessão com o banco
│   └── config.py             # Variáveis de ambiente
├── alembic/                  # Controle de versão do banco
├── .env.example              # Variáveis de ambiente
└── requirements.txt          # Dependências do projeto
```

---

## ☁️ Banco de Dados

- **Ambiente de desenvolvimento:** PostgreSQL local
- **Migrações:** Alembic controlando todas as alterações no schema
- **Conexão:** via variável `DATABASE_URL` no `.env`

---

## 🔐 Segurança

- Tokens JWT para autenticação
- Hashing de senhas com `passlib`
- CORS configurado para consumo seguro pelo frontend
- Projeção de rotas sensíveis (ex: `/users/me`, `/checkin`) com autenticação obrigatória

---

## 🚀 Deploy

*(Previsto para MVP funcional)*

- Pipeline de deploy automático com GitHub Actions
- Deploy contínuo para Render, Railway ou Fly.io
- Banco de dados provisionado junto ao backend
- Monitoramento básico com logs e alertas (futuramente Prometheus + Grafana)

---

## 🧪 Testes e Validação

- Testes manuais via Swagger e HTTPie
- Validações automáticas com Pydantic nos schemas
- Planejamento futuro: cobertura de testes com Pytest

---

## 📎 Observações

- A aplicação está pronta para escalar com containers (Dockerfile disponível futuramente)
- Código modularizado para facilitar manutenção e extensão
- Toda a infraestrutura foi pensada para simplicidade e agilidade de entrega no MVP


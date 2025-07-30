
# ⚙️ BUILD – Como Rodar o Projeto Localmente

Este projeto é dividido em três repositórios:

- [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs) – Documentação e design
- [`checkin-front`](https://github.com/gabrielalimasotero/checkin-frontend) – Aplicação mobile (React Native + Expo)
- [`checkin-back`](https://github.com/CHMFC/checkin-back) – API backend (FastAPI + PostgreSQL)

---

## 🧱 1. Clonando os repositórios

```bash
# Clonar os três repositórios
git clone https://github.com/gabrielalimasotero/checkin-docs.git
git clone https://github.com/gabrielalimasotero/checkin-frontend
git clone https://github.com/CHMFC/checkin-back.git
```

---

## 🧪 2. Rodando o Backend (FastAPI)

### Pré-requisitos

- Python 3.9+
- PostgreSQL
- pip + venv

### Instruções

```bash
cd checkin-back

# Criar e ativar o ambiente virtual
python -m venv venv
source venv/bin/activate  # Windows: .\venv\Scripts\activate

# Instalar dependências
pip install -r requirements.txt

# Configurar variáveis de ambiente
cp .env.example .env
# Edite o .env com suas credenciais

# Aplicar migrações
alembic upgrade head

# Rodar servidor
uvicorn app.main:app --reload
```

A API estará acessível em: http://127.0.0.1:8000  
Documentação: `/docs` (Swagger) ou `/redoc`

---

## 📱 3. Rodando o Frontend (React Native + Expo)

### Pré-requisitos

- Node.js
- Expo Go (no celular)
- NPM ou Yarn

### Instruções

```bash
cd checkin-front

# Instalar dependências
npm install  # ou yarn install

# Rodar o servidor Expo
npx expo start
```

- Escaneie o QR Code com o app Expo Go no celular
- Ou pressione `a` (Android) ou `i` (iOS) no terminal para rodar em emulador

---

## 📘 4. Visualizando a Documentação

O repositório `checkin-docs` contém toda a documentação do projeto.  
Você pode navegar pelos arquivos `.md` localmente ou hospedar com GitBook/Docsify.

```bash
cd checkin-docs
# Abrir manualmente os arquivos ou usar uma ferramenta de visualização Markdown
```

---

## 🧠 Dicas Adicionais

- Use `.env.example` como base para suas variáveis de ambiente
- O backend deve estar rodando para que o frontend funcione corretamente
- Consulte o README de cada repositório para detalhes específicos

---

## 📞 Suporte

Dúvidas ou problemas? Entre em contato com a equipe via Issues ou pelos canais internos.

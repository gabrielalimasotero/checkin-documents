
# ⚙️ BUILD – How to Run the Project Locally

This project is divided into three repositories:

- [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs) – Documentation and design
- [`checkin-frontend`](https://github.com/gabrielalimasotero/checkin-frontend) – Mobile application (React + TypeScript + Vite)
- [`checkin-backend`](https://github.com/CHMFC/checkin-backend) – API backend (FastAPI + PostgreSQL)

---

## 🧱 1. Cloning the repositories

```bash
# Clone the three repositories
git clone https://github.com/gabrielalimasotero/checkin-docs.git
git clone https://github.com/gabrielalimasotero/checkin-frontend
git clone https://github.com/CHMFC/checkin-backend.git
```

---

## 🧪 2. Running the Backend (FastAPI)

### Prerequisites

- Python 3.8+
- PostgreSQL
- pip + venv

### Instructions

```bash
cd checkin-backend

# Create and activate virtual environment
python -m venv venv
source venv/bin/activate  # Windows: .\venv\Scripts\activate

# Install dependencies
pip install -r requirements.txt

# Configure environment variables
cp .env.example .env
# Edit .env with your credentials

# Run server (choose one)
python run.py
# or
uvicorn app.main:app --reload
```

The API will be accessible at: http://127.0.0.1:8000
Documentation: `/docs` (Swagger) or `/redoc`

---

## 📱 3. Running the Frontend (React + TypeScript + Vite)

### Prerequisites

- Node.js 18+ or Bun
- NPM, Yarn, or Bun

### Instructions

```bash
cd checkin-frontend

# Install dependencies
npm install  # or yarn install or bun install

# Environment
# Create `.env.local` and set at least:
# VITE_API_BASE_URL=http://127.0.0.1:8000
# VITE_SUPABASE_URL=... (see SUPABASE_SETUP.md in frontend)
# VITE_SUPABASE_ANON_KEY=...

# Run development server
npm run dev  # or yarn dev or bun dev
```

The application will be accessible at: http://localhost:5173
Ensure the backend is running and `VITE_API_BASE_URL` points to it.

---

## 📘 4. Viewing the Documentation

The `checkin-docs` repository contains all project documentation.  
You can navigate through the `.md` files locally or host with GitBook/Docsify.

```bash
cd checkin-docs
# Open files manually or use a Markdown viewing tool
```

---

## 🧠 Additional Tips

- Use `.env.example` as a base for your environment variables
- The backend must be running for the frontend to work properly
- Check each repository's README for specific details
- The frontend is optimized for mobile devices but works on desktop browsers

---

## 📞 Support

Questions or problems? Contact the team via Issues or internal channels.

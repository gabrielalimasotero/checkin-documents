# 📍 CheckIn – Social Venue Check-in Platform

**CheckIn** is a modern mobile-first social platform that connects people through shared venue experiences. The app allows users to check into restaurants, bars, and venues while discovering new places and connecting with friends.

Developed as a React-based mobile application, CheckIn answers the practical questions:
- _"Where should I go out today?"_
- _"Who should I go with?"_

---

## 🎯 Project Objectives

- Reduce decision time for going out
- Help users find active, safe places with good vibes
- Enable more natural social encounters connected to the moment
- Support venues in organizing flow, reservations, and promotion
- Provide a smooth, light, and personalized experience

---

## 🧱 Repository Structure

This project is organized in the following repositories:

| Repository        | Purpose                                           |
|--------------------|---------------------------------------------------|
| `checkin-documents/` | General documentation, design, requirements and architecture |
| `checkin-frontend/`  | Mobile application (React + TypeScript + Vite)   |
| `checkin-backend/`   | API backend (FastAPI + PostgreSQL)               |

---

## 🧩 Architecture and Design

The application is structured in a client-server architecture with a focus on mobile-first design and modularity.

📌 Diagrams available in:
`checkin-documents/04-architecture`

- **Frontend:** React application with TypeScript, Vite, and Shadcn/ui
- **Backend:** RESTful API with FastAPI, JWT (issued by email), PostgreSQL
- **Design System:** Visual identity based on dark blue (#084d6e), white, black
- **Mobile-First:** Optimized for mobile devices with responsive design

---

## 📚 Documentation Navigation

Complete documentation is available in [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs), organized by theme:

- `01-general/` → Proposal, problem, personas, differentiation
- `02-requirements/` → User stories, criteria, backlog
- `03-design/` → Visual identity, wireframes, user flow
- `04-architecture/` → Frontend, backend, data model, C4
- `05-plan/` → Roadmap, sprints, roles
- `06-tests/` → Validation strategies, metrics
- `99-appendices/` → References and complementary materials

---

## 🔗 Important Resources

- 🗂️ [Task Board on Trello (Scrumban)](https://trello.com/b/97MLpiuS/checkin-scrumban)
- 🎨 [Figma Prototypes](#) *(add link)*
- 📎 Backend interactive API docs (Swagger): `http://localhost:8000/docs`

---

## ⚙️ How to Run the Project Locally

1) Backend (FastAPI)
```bash
cd ../checkin-backend
cp env.example .env
python -m venv venv && source venv/bin/activate
pip install -r requirements.txt
python run.py
```

2) Frontend (React)
```bash
cd ../checkin-frontend
npm install
cp .env.example .env.local # se existir; senão, crie e defina VITE_* conforme README
npm run dev
```

3) Documents
```bash
cd ../checkin-documents
# See this README and BUILD.md for details
```

---

## 🤝 Contribution

External contributions are welcome but curated by the original team.
Guidelines: [CONTRIBUTING.md](./CONTRIBUTING.md)

---

## 👥 Team

- Gabriela Lima Sotero *(Team Leader, PO, Designer)*
- Henrique Fontaine *(Technical Architect and Backend Dev)*
- João Pedro de Albuquerque Maranhão Marinho *(Frontend)*
- João Victor Oliveira Santos *(Backend)*
- Lucas Emmanuel Gomes de Lucena *(Modeling and Infrastructure)*
- Lucas Luis de Souza *(Frontend)*

---

## 📄 License

This project is licensed under the [MIT License](./LICENSE).

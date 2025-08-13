
# 🤝 How to Contribute to CheckIn

Before you start, thank you for considering contributing to CheckIn!

This project is maintained by a core team and external contributions are **welcome**, but go through **curation and review** to ensure alignment with the overall vision.

---

## ✅ What you can contribute

- Bug fixes
- Documentation improvements (e.g., README, requirements, architecture)
- Feature suggestions or usability improvements
- Refactoring that maintains existing functionality
- Use cases, UX feedback, and blind spots

---

## 🛠️ How to contribute – Step by step

1. **Fork** this repository
2. Create a branch with a clear name:

```bash
git checkout -b feat/venue-suggestions
```

3. Make your changes
   - Follow the stacks defined in README (FastAPI + PostgreSQL on backend; React + TypeScript + Vite + Tailwind + shadcn/ui on frontend)
   - Keep API contracts in sync (see Venue shape in documentation). If backend changes types (e.g., `latitude`/`longitude` to string), update frontend typings and docs.
4. Commit your changes with a descriptive message:

```bash
git commit -m "feat: add venue suggestions based on user preferences"
```

5. Push your branch:

```bash
git push origin feat/venue-suggestions
```

6. Open a **Pull Request (PR)** to the `main` branch of the original repository

---

## 📌 Rules and best practices

- Write clear and objective commits (`feat`, `fix`, `docs`, `refactor`, etc.)
- Always explain in the PR **what was changed** and **why**
- Prefer smaller, iterative changes
- For large suggestions or route changes, **open an issue first**
- Any code should be functional and follow existing patterns

---

## 🧪 Testing and verification

Before submitting a PR, make sure that:

- The project still runs locally (see `BUILD.md`)
- Your contribution doesn't break existing endpoints or app flow
- Documentation has been updated (if necessary)
- Code follows the design system and component patterns
- TypeScript types are properly defined
  - Especially API interfaces (e.g., `Venue`):
    - `latitude`/`longitude`: `string | null`
    - `price_range`: `string | null`
    - `features`: `string | null` (string with values separated by comma/semicolon)

---

## 🎨 Design System Guidelines

When contributing to the frontend:

- Use the established design tokens (colors, spacing, typography)
- Follow the Shadcn/ui component patterns
- Maintain mobile-first responsive design
- Use the primary color palette (#084d6e, white, black)
- Follow the established folder structure and naming conventions

---

## 💬 Feedback and Discussions

Want to contribute ideas, flows, or design?  
Open an issue with the `discussion` type or send your visual proposal to the team's internal channels.

---

## 👥 Team Contact

- Gabriela Lima Sotero – PO and design
- Henrique Fontaine – Technical architect
- Main documentation repository: [`checkin-docs`](https://github.com/gabrielalimasotero/checkin-docs)

---

## 📄 License

By contributing to this project, you agree to the terms of the [MIT License](./LICENSE).


# 🧱 Component Structure – Frontend (React + TypeScript)

The CheckIn application is developed in **React with TypeScript and Vite**, with a modular structure and clear separation between visual, logic, and data layers.

---

## 📁 General Folder Structure

```
checkin-frontend/
├── public/              # Static assets, images, icons
├── src/
│   ├── components/      # Reusable components (buttons, cards, badges)
│   │   ├── ui/         # Shadcn/ui components
│   │   ├── MainNavigation.tsx
│   │   ├── TopHeader.tsx
│   │   └── ...
│   ├── pages/          # Main application pages
│   │   ├── Welcome.tsx
│   │   ├── Home.tsx
│   │   ├── CheckIn.tsx
│   │   ├── Social.tsx
│   │   ├── Profile.tsx
│   │   └── ...
│   ├── contexts/       # React contexts (auth, user)
│   ├── hooks/          # Custom React hooks
│   ├── lib/            # Utilities and design system
│   ├── App.tsx         # Main application component
│   └── main.tsx        # Application entry point
├── package.json
├── vite.config.ts
└── tailwind.config.ts
```

---

## 📦 Reusable Components

| Component        | Main Usage                                           |
|------------------|------------------------------------------------------|
| `Card`           | Standard card component with variants                |
| `Button`         | Primary, secondary, outline, and ghost buttons      |
| `Badge`          | Status indicators, tags, and labels                 |
| `Avatar`         | User profile pictures or initials                    |
| `Tabs`           | Tab-based navigation and content switching           |
| `Dialog`         | Modals for confirmations and forms                   |
| `Toast`          | Notification messages and feedback                   |

---

## 🔁 Navigation

The app uses **React Router DOM** with a mobile-first approach:

- **Main Routes:**
  - `/welcome` - Authentication and onboarding
  - `/home` - Main dashboard with tabs
  - `/checkin` - Venue check-ins and management
  - `/social` - Social network and connections
  - `/profile` - User profile and settings
  - `/notifications` - Event reminders and updates
  - `/messages` - Direct messaging

- **Tab Navigation:**
  - Home page has three tabs: "For You", "Network", "Explore"
  - CheckIn page has four tabs: "Geral", "Histórico", "Ativo", "Cardápio"

---

## ☁️ Integrations

- **API Backend (FastAPI):**
  - Authentication (login, registration, tokens)
  - Events and invitations
  - Venues and check-ins
  - Ratings and notifications

- **Design System:**
  - Shadcn/ui components
  - Tailwind CSS for styling
  - Custom design tokens and variants

- **State Management:**
  - React Context for global state
  - React Query for server state
  - Local state with useState and useReducer

---

## 📐 Styling and Responsiveness

- **Tailwind CSS** for utility-first styling
- **Shadcn/ui** for consistent component library
- **Mobile-first** responsive design
- **Design tokens** for consistent spacing, colors, and typography
- Support for light and dark modes based on the blue palette (#084d6e)

---

## 🧪 Testing and Validation

- **Development:** Vite dev server with hot reload
- **Build:** Vite for production builds
- **Linting:** ESLint for code quality
- **Type Checking:** TypeScript for type safety
- **Design Validation:** Custom script to check design system consistency

---

## 🎨 Design System

### Colors
- **Primary:** #084d6e (Dark Blue)
- **Background:** #ffffff (White)
- **Text:** #000000 (Black)
- **Muted:** #6b7280 (Gray)

### Typography
- **Headings:** Ubuntu font for specific titles
- **Body:** Helvetica for all content
- **Responsive:** Mobile-optimized font sizes

### Components
- **Cards:** Standard, compact, elevated variants
- **Buttons:** Primary, secondary, outline, ghost
- **Navigation:** Bottom navigation with icons
- **Forms:** Consistent input styling and validation

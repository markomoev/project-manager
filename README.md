# Project Manager

A modern, responsive project management app built with **Vue 3** and **Vite**, designed to help you organize and track projects seamlessly.

---

## Features

- Add, view, and manage projects
- Responsive card layout (up to 4 per row on desktop, fully mobile-friendly)
- 3D hover effects for project cards
- Status indicator and link for each project
- Data persistence using localStorage

---

## Tech Stack

- [Vue 3](https://vuejs.org/)
- [Vite](https://vitejs.dev/)
- [Tailwind CSS](https://tailwindcss.com/)

---

## Getting Started

1. **Install dependencies:**

   ```sh
   npm install
   ```

2. **Run the development server:**

   ```sh
   npm run dev
   ```

3. **Build for production:**

   ```sh
   npm run build
   ```

---

## Usage

- Click **Add Project** to create a new project card.
- Each card shows the project name, description, status, and a link.
- Cards and project details are stored in your browser’s localStorage.
- Click on project cards to open a detailed popup with options to edit or delete.

---

## Version History

### v1.0

- Basic project adding and listing functionality.

### v1.1

- Added project detail popup modal.

### v1.2 (latest)

- Improved popup buttons with cohesive, subtle styling aligned with overall design.
- Unique, easily accessible delete button with larger icon.
- Responsive button layouts for all screen sizes.
- Enhanced editing experience with clear states and smooth interactions.

# CTPVFX — CUT.POINT Website

React + Vite + Three.js landing page for CUT.POINT, a VFX/film editing studio.

## Stack
- React 19, Vite 8
- Three.js via @react-three/fiber + @react-three/drei (3D hero)
- Pure CSS (no Tailwind) — all styles in `src/index.css`
- EmailJS for contact form

## Design System
- **Style:** Dark cyberpunk / cinematic — think film-editing terminal meets VFX pipeline
- **Palette:** `--black: #020408`, `--white: #e8f4ff`, `--orange: #00d4ff` (cyan, mislabeled), `--accent2: #7b2fff` (purple), `--accent3: #ff2d6b` (red)
- **Fonts:** Bebas Neue (display headings), DM Sans (body), DM Mono (labels/tags/code)
- **Motion:** CSS animations for hero fade-in, scroll reveal, glitch, scanline, particles canvas

## UI/UX Skill

This project uses the **UI/UX Pro Max** skill (`.claude/skills/ui-ux-pro-max/`).

When working on any UI/UX task — new sections, components, color choices, typography, animations, accessibility — consult the skill:
- Read `.claude/skills/ui-ux-pro-max/SKILL.md` for design guidelines
- The relevant tech stack data is in `.claude/skills/ui-ux-pro-max/data/stacks/react.csv`
- Run the design system generator for new pages/sections:
  ```
  python3 .claude/skills/ui-ux-pro-max/scripts/design_system.py
  ```

### Key constraints from the skill for this project:
- Stack: **React** — use the react.csv stack patterns
- Style category: **Dark Mode + Cinematic/Brutalism hybrid**
- Always check CRITICAL rules: color contrast ≥ 4.5:1, touch targets ≥ 44×44px, focus states visible
- Animation: use `transform`/`opacity` only (not `width`/`height`), respect `prefers-reduced-motion`
- Performance: lazy-load images, use WebP, reserve space for async content

## File Structure
```
src/
  App.jsx       — all JS logic + JSX markup (single-file app)
  index.css     — all styles (CSS custom properties + BEM-ish classes)
  App.css       — Vite scaffolding remnant (mostly unused)
  assets/       — static assets (model.zip, etc.)
public/         — static files served at root
index.html      — entry point
```

## Dev Commands
```bash
npm run dev      # start dev server
npm run build    # production build
npm run preview  # preview build
```

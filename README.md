# gfierro.dev

Personal portfolio site for Gerard Fierro — Senior Software Engineer / Front End.

## Stack

- [Astro](https://astro.build) — static-first site build, ships minimal JS
- [Vue 3](https://vuejs.org) — islands for interactive pieces (nav, theme toggle, project filter)
- [Tailwind CSS v4](https://tailwindcss.com) — styling, mobile-first responsive design

## Develop

```bash
npm install
npm run dev
```

## Build

```bash
npm run build
npm run preview
```

## Deploy

Pushes to `main` build and deploy automatically to GitHub Pages via
[`.github/workflows/deploy.yml`](.github/workflows/deploy.yml).

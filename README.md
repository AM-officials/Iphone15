# iPhone 15 Interactive Product Page Clone

An interactive, high‑fidelity, scroll / gesture driven product showcase inspired by Apple's iPhone 15 marketing page. Built with React 18, Vite, Three.js (via react-three-fiber + drei), GSAP, and Tailwind CSS. Animations, 3D model orchestration, and subtle UI polish aim to mirror the feel of Apple's site while remaining fully hackable.

> Not affiliated with Apple. This is an educational / portfolio recreation.

---

## Quick Start

```bash
git clone https://github.com/AM-officials/Iphone15.git
cd Iphone15
npm install
npm run dev
```

Open http://localhost:5173 (default Vite port).

Build for production:

```bash
npm run build
npm run preview
```

---

## Features

- React + Vite fast dev environment (ESM, HMR)
- 3D device / scene rendering (three, @react-three/fiber, @react-three/drei)
- Scroll & timeline driven animation choreography (GSAP + optional ScrollTrigger patterns)
- Tailwind CSS utility styling + custom design tokens
- Progressive component reveal & section transitions
- Optimized asset loading (lazy mounts, suspense, preloading strategies)
- Error / performance instrumentation prepared with Sentry (@sentry/react + @sentry/vite-plugin)
- Modular component breakdown for quick experimentation
- Clean linting setup (ESLint + React + Hooks + Refresh plugin)

---

## Tech Stack

| Purpose          | Library / Tool |
|------------------|----------------|
| Framework        | React 18 |
| Bundler / Dev    | Vite 5 |
| 3D Layer         | three, @react-three/fiber, @react-three/drei |
| Animations       | GSAP (@gsap/react) |
| Styling          | Tailwind CSS + PostCSS + Autoprefixer |
| Quality          | ESLint (React, Hooks, Refresh) |
| Monitoring       | Sentry (browser + build plugin) |
| Language         | JavaScript (ESM) |

---

## Scripts

| Script      | Description |
|-------------|-------------|
| npm run dev | Start dev server (Vite) |
| npm run build | Production build |
| npm run preview | Preview build locally |
| npm run lint | Lint all JS / JSX |

---

## Directory Layout (root)

```
.
├── index.html
├── package.json
├── package-lock.json
├── src/                 # All application source (see below)
├── public/              # Static assets served as-is
├── tailwind.config.js
├── postcss.config.js
├── vite.config.js
├── .eslintrc.cjs
└── .gitignore
```

Inside `src/` (typical organization — adjust if you rename):

```
src/
├── components/          # Reusable presentational + interactive pieces
├── sections/            # Page-level vertical slices (hero, features, colors, etc.)
├── hooks/               # Custom hooks (scroll progress, viewport metrics)
├── models/              # 3D model loaders, glTF setup, materials
├── lib/                 # Helpers (animation timelines, constants)
├── styles/              # Global or Tailwind extension files (if any)
├── config/              # Sentry / feature flags (optional)
└── main.jsx             # App root / ReactDOM.createRoot
```

If your internal naming diverges, treat the above as intent.

---

## Environment & Configuration

Sentry (optional) – create a `.env` or export env vars before build:

```
SENTRY_AUTH_TOKEN=...
SENTRY_ORG=your_org
SENTRY_PROJECT=iphone15
SENTRY_DSN=https://examplePublicKey.ingest.sentry.io/projectId
```

Typical integration (conceptually):

```js
import * as Sentry from '@sentry/react';

Sentry.init({
  dsn: import.meta.env.VITE_SENTRY_DSN,
  tracesSampleRate: 0.3,
  integrations: [],
});
```

Add `VITE_` prefix for variables you need exposed to the client:

```
VITE_SENTRY_DSN=https://...
```

Tailwind config lives in `tailwind.config.js`. PostCSS pipeline configured via `postcss.config.js`.

---

## Animation & Scroll Architecture

1. Each major section defines its own GSAP timeline.
2. Scroll progress can be mapped to normalized values (0..1) to drive:
   - 3D camera position / rotation
   - Material / color transitions
   - Opacity + translate for text blocks
3. Heavy 3D elements lazy‑mount once scrolled near viewport to keep TTFB / initial FPS stable.
4. Where possible, offload static effects to CSS (e.g., simple fades) and reserve GSAP for orchestrated multi‑target sequences.
5. React Three Fiber:
   - Use `<Suspense fallback={...}>` for model loads.
   - Keep mesh counts low; merge geometry or bake lighting if complexity grows.
   - Use `drei` helpers (e.g. `Environment`, `OrbitControls`—or custom locked controls).

---

## Performance Notes

- Vite handles code splitting automatically via dynamic imports.
- Tree-shake unused drei helpers (import only what you use).
- Compress and DRACO / Meshopt optimize large `.glb` models.
- Use `useMemo` / `useCallback` for stable references in animation hooks.
- Monitor bundle size with `vite build --mode analyze` (plugin optional).
- For very animation-heavy frames, consider `gsap.ticker.lagSmoothing(0)` only when diagnosing.

---

## Adding / Modifying Sections

1. Create a new component under `src/sections/YourSection`.
2. Export it and compose inside the main page layout (e.g., `App` or `Page` component).
3. Assign a data attribute or id for scroll tracking.
4. Define an animation hook (e.g. `useSectionTimeline`) returning a GSAP timeline synced to scroll progress.

---

## 3D Model Integration

- Place processed `.glb` in `public/models/` (loads via root-relative URL `/models/phone.glb`).
- Use `useGLTF` (from `drei`) to load.
- Abstract model presentation logic into `models/PhoneModel.jsx`.
- Drive dynamic color via material uniforms or multiple material variants.

---

## Error & Crash Monitoring (Sentry)

Build-time plugin (`@sentry/vite-plugin`) can auto-upload source maps when env vars are present. Without credentials, it silently skips. Leave it in; no config changes needed for local experimentation.

---

## Linting

Run:

```bash
npm run lint
```

Rules:
- No unused React imports (React 18 + JSX transform)
- Hooks exhaustive deps enforced
- No unused disable directives (`--report-unused-disable-directives`)

---

## Deployment

Any static host (Vercel, Netlify, Cloudflare Pages, GitHub Pages) works:

```bash
npm run build
# dist/ contains static assets
```

Upload `dist/`. If using Sentry release process, ensure env vars are present during build.

---

## Roadmap (Living)

- Enrich device color / material transitions
- Add accessibility pass (reduced motion, proper alt text)
- Introduce mobile touch gesture parallels to scroll states
- Performance budget instrumentation (web vitals overlay)
- Light / dark adaptive background blends
- Unit tests for animation mappings (utility functions)
- Add snapshot/perf testing harness (optional)

---

## Contributing

Issues & PRs welcome. Suggested flow:

1. Fork / branch: `feat/your-improvement`
2. Keep commits atomic (`feat: add color selector panel`)
3. Run lint before pushing
4. Describe rationale & screenshots / GIFs if UI visible

Please avoid adding heavyweight libraries without discussion.

---

## FAQ

Q: Is any Apple asset used directly?  
A: No proprietary assets should ship. Use self-created or openly licensed 3D and media.

Q: Can I reuse this code commercially?  
A: Yes, under the license below (avoid infringing Apple trademarks or trade dress).

Q: Models look heavy—why?  
A: Tradeoff between fidelity and performance; you can simplify or decimate meshes.

---

## License

MIT. Do whatever you like within responsible bounds. Attribution appreciated but not required.

```
MIT License

Copyright (c) 2025 AM-officials

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

[Standard MIT text continues...]
```

(If you add a standalone LICENSE file, you can remove the inline block.)

---

## Credits

- three / react-three-fiber / drei teams
- GSAP for timeline ergonomics
- Tailwind CSS for rapid styling
- Sentry for instrumentation tooling

---

## Disclaimer

This is a fan-made educational recreation for practicing modern frontend + 3D integration techniques. All iPhone / Apple trademarks belong to Apple Inc.

---

Enjoy exploring the code. Open an issue if something is unclear or you have an optimization idea. Happy hacking!

# Bavigadda Naresh - 3D Interactive Portfolio

A high-performance, single-file 3D interactive portfolio website built for **Bavigadda Naresh** (Python Full Stack Developer). Combines an immersive WebGL particle constellation and wireframe geometric spatial background with readable glassmorphic editorial typography, interactive 3D tilt cards, and a robust direct contact system.

---

## User Review & Critical Decisions

> [!IMPORTANT]
> The following architectural decisions have been aligned based on user preferences and requirements:

- **Confirmed Decision 1 (Contact Channel)**: The contact form will default to verified zero-loss communication workflows (direct "Send with Gmail" prefilled composer, standard mailto client, and 1-click address copy). A top-level `FORM_ENDPOINT` variable is cleanly declared at the top of the script so adding a Formspree/FormSubmit URL immediately activates direct JSON transmission with animated success feedback.
- **Confirmed Decision 2 (Resume Action)**: A dedicated "View Resume" button will be integrated in both the top navigation bar and hero CTA cluster, opening a clean, printer-friendly CV modal reflecting all exact academic and professional credentials.
- **Confirmed Decision 3 (3D Particle Density & Performance)**: A rich network of 1,500+ glowing nodes and dynamic connecting lines styled in electric cyan (`#62d4e8`) with warm amber accents (`#ffb347`), featuring camera flight along a smooth 3D spline curve synced to scroll depth, section wireframe objects, cursor repulsion, and graceful fallback for systems with hardware acceleration disabled or reduced motion preferences.
- **Confirmed Decision 4 (Profile Photography)**: The user's uploaded portrait (`/public/assets/aistudio/linkden_c25d0c82.jpeg`) will be displayed in the hero section with a crisp circular frame, subtle cybernetic boundary ring, and fallback SVG placeholder styling.

---

## 1. Overview & Core Concept

- **What It Does**: Presents Bavigadda Naresh’s technical identity, projects, internship track record, skills, and academic qualifications through a futuristic yet legible 3D web experience.
- **Target Audience**: Engineering hiring managers, technical recruiters, and collaborators seeking a Python full-stack engineer with hands-on systems, ML, and IoT competence.
- **Core Value**: Combines cutting-edge 3D WebGL visual choreography (camera spline flight, cursor repulsion, wireframe geometric section anchors) with rock-solid semantic accessibility, zero hallucinated facts, and high readability.

---

## 2. User Experience & Visual Design

### Key User Flows
1. **Hero Entry**: The user lands on an immersive dark space (`#070a11`). The 3D camera slowly drifts through an illuminated cyan/amber neural constellation. A custom magnetic cursor tracks mouse movement. Naresh's professional portrait, headline, role, and immediate CTAs ("Contact Me", "View Projects", "Resume") fade in with crisp typographic presence.
2. **Smooth Flight Scroll**: As the user scrolls:
   - The 3D camera glides along a smooth Bezier 3D curve through the particle space.
   - The field of view (FOV) subtly widens with scroll velocity and settles smoothly.
   - Each section brings a dedicated wireframe 3D object (torus knot, icosahedron, nested rings, octahedron, helix of points, dodecahedron, concentric rings) into focus, scaling and rotating as the camera approaches.
   - Section headers decode character-by-character using an editorial scramble text effect with an animated amber underline wipe.
3. **Interactive Project Exploration**: The two verified projects (*Phishing Website Detection using ML* and *Smart Irrigation System using IoT*) are presented in 3D perspective tilt cards with cursor-following radial glow highlights, tech stack badges, and key architectural bullet points.
4. **Experience & Education Timeline**: Chronological vertical timeline layout displaying internships (Brain O Vision, IIDT & Blackbuck Engineers, Ignite Institute) and education (B.Tech CSE at Indira Institute of Tech & Sciences, Intermediate at Sastra JC, SSC at Prathibha Vikas) with clean typographic separators.
5. **Contact & Transmission**:
   - Form fields (Name, Email, Message) with real-time validation and active focus glow.
   - If `FORM_ENDPOINT` is unconfigured: Transparently displays fallback actions with "Send with Gmail" (prefilled compose URL), "Other Email App", and "Copy Address".
   - If `FORM_ENDPOINT` is set: Submits JSON via `fetch()`, displays animated checkmark success panel with "Send another message" reset option.

### Visual Identity & Theme
- **Dominant Neutral (60%)**: Deep cosmic navy `#070a11` with translucent structural cards (`rgba(15, 23, 42, 0.72)` / `backdrop-blur-md` with `1px solid rgba(98, 212, 232, 0.15)`).
- **Secondary Tone (30%)**: Vibrant electric cyan `#62d4e8` for network nodes, connecting lines, section highlights, and code tags.
- **Focal Accent (10%)**: Warm solar amber `#ffb347` for primary action buttons, underline wipes, active states, and glowing particle accents.
- **Typography**:
  - Headings: `Bricolage Grotesque` (Google Fonts) with tight optical tracking.
  - Body Prose: `Hanken Grotesk` (Google Fonts) for crisp, readable descriptions.
  - Labels & Metadata: `IBM Plex Mono` (Google Fonts) for technical credentials, dates, and metrics.
- **Accessibility & Motion Guards**: Full `prefers-reduced-motion` compliance (replaces 3D camera zoom/particles with subtle static nebula gradient and turns off aggressive text scramble). WebGL failure gracefully switches to pure CSS visual gradient backdrop.

---

## 3. Key Product Decisions & Trade-Offs

### Decision 1: Single-File Architecture with Zero Build Step
- **Chosen Approach**: All HTML, CSS, Three.js orchestration, and event handlers bundled in `index.html` (alongside Vite compatibility for instant preview and deployment).
- **Why**: Meets the user's explicit specification ("Build me a single-file, professional 3D interactive portfolio website (one index.html, no build step)"). Can be downloaded or hosted on GitHub Pages, Netlify, Cloud Run, or any static server without build pipelines.
- **Alternatives Considered**: Multi-component React build. While React is installed in the repo, fulfilling the user's exact "one index.html, no build step" request with pinned CDN scripts allows the file to be opened directly from disk or served as a standalone artifact. We will also maintain `src/App.tsx` and Vite configuration to ensure seamless preview in both environments.

### Decision 2: 3D Object Choreography & Viewport Occlusion
- **Chosen Approach**: Position 3D wireframe objects in depth ($Z$) along the camera flight path, offset slightly toward the right or background behind content containers with measured opacity so text maintains WCAG AAA contrast ratio ($\ge 7:1$).
- **Why**: Keeps spatial depth tangible without obscuring essential text readability.
- **Alternatives Considered**: Fullscreen models centered over copy, which causes severe legibility degradation.

### Decision 3: Formspree/Custom Endpoint + Verified Fallback Engine
- **Chosen Approach**: Provide both modes in a single unified component: detect whether `FORM_ENDPOINT` contains a valid URL string. If empty, display an interactive fallback panel with direct Gmail composer URL, mailto, and clipboard copy. If configured, send a POST request with JSON payload.
- **Why**: Guarantees zero lost messages regardless of whether the user has set up a third-party backend yet.

---

## 4. Technical Architecture & Data Strategy

```
┌────────────────────────────────────────────────────────────────────────────┐
│                             BROWSER VIEWPORT                               │
├────────────────────────────────────────────────────────────────────────────┤
│  [ Fixed Top Bar: Logo | Nav Links (Hero, About, Skills, Projects, Exp) ] │
│  [ Scroll Progress Line (Amber) ]                                          │
├────────────────────────────────────────────────────────────────────────────┤
│  LAYER 0: Fixed Three.js WebGL Canvas (Full Viewport)                      │
│   ├── 1,500+ Node Particle BufferGeometry (Cyan + Amber highlights)        │
│   ├── Dynamic Distance-Based Line Mesh (BufferGeometry with line pairs)    │
│   ├── 7 Section Wireframe Meshes along 3D Curve Flight Path:               │
│   │   ├── S1 Hero: Torus Knot                                              │
│   │   ├── S2 About: Icosahedron                                            │
│   │   ├── S3 Skills: Concentric Rings                                      │
│   │   ├── S4 Projects: Octahedron                                          │
│   │   ├── S5 Experience: Point Helix                                       │
│   │   ├── S6 Education: Dodecahedron                                       │
│   │   └── S7 Contact: Concentric Nested Spheres / Rings                    │
│   └── Camera Spline Controller (Lerped Scroll Progress + Mouse Tilt)       │
├────────────────────────────────────────────────────────────────────────────┤
│  LAYER 1: Semantic DOM Content (Z-Index 10, Glassmorphic Panels)           │
│   ├── #hero: Portrait, Name, Role, Location, Objective, CTAs, Socials      │
│   ├── #about: Background, Objective statement, Key Strengths               │
│   ├── #skills: Languages, Frameworks, Frontend, DB, Tools, Hardware/IoT    │
│   ├── #projects: Phishing ML Detection Card + Smart Irrigation IoT Card    │
│   ├── #experience: Timeline of 3 Internships (Brain O Vision, IIDT, Ignite)│
│   ├── #education: B.Tech (JNTUK), Intermediate (Sastra), SSC (Prathibha)   │
│   ├── #contact: Form / Direct Email Fallback System + Copy Action          │
│   └── Modal: Clean printable Resume view                                   │
├────────────────────────────────────────────────────────────────────────────┤
│  LAYER 2: Custom Cursor (Dot + Lagging Smooth Ring, Touch-Hidden)          │
└────────────────────────────────────────────────────────────────────────────┘
```

### Component & State Mapping

| Section / Feature | Primary Interactive State | Event Handlers & Behaviors |
| :--- | :--- | :--- |
| **Top Navigation** | `activeSection`, `scrollProgress` | Smooth scroll into view, dynamic active indicator, scroll progress bar updates on `window.scroll`. |
| **3D Canvas** | `cameraPosition`, `mouseRepel`, `fovWarp` | `pointermove` calculates normalized coordinates for camera tilt & node repulsion; scroll listener computes spline parameter $t \in [0, 1]$. |
| **Section Reveal** | `inView` via `IntersectionObserver` | Triggers character scramble text decode, clip-path wipe, and amber underline drawing. |
| **Project Cards** | `rotX`, `rotY`, `glowX`, `glowY` | `mousemove` calculates 3D transform `perspective(1000px) rotateX(...) rotateY(...)` and updates radial gradient spotlight. |
| **Contact Form** | `isSubmitting`, `isSent`, `errors` | Inline validation on submit; POST to `FORM_ENDPOINT` if provided, or display Gmail / mailto fallback modal. |
| **Resume Modal** | `isOpen` | Opens formatted printable credential summary; `ESC` and click outside dismiss. |
| **Custom Cursor** | `cursorPos`, `isHoveringInteractive` | Lerped trailing ring scales up on `a`, `button`, and input hovers; hidden on mobile. |

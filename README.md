# Gold Frameworks

A collection of attempts to build ideal fullstack web frameworks aligned with my personal philosophy and real-world experience.

The goal is to create solutions optimized for the most common use cases I’ve encountered across my career. These frameworks deliberately avoid techniques and technologies that only benefit a minority of projects.

## Patterns (What to do)

### Separate Backend and Frontend
Mixing backend and frontend can feel more cohesive at first, but most projects end up splitting them anyway. Full integration via TypeScript is possible, but the main benefit — shared types — can be achieved through cleaner, less coupling-heavy approaches. In my experience, the mixed model consistently introduces unnecessary drawbacks.

### Frontend Served Through the Backend
All data and assets should be served from a single system. Hosting the frontend as static files from a separate service often leaks sensitive usage information and complicates security. Serving everything through the backend allows centralized control, simpler authentication, and easier lockdown of both layers.

### Generated Typed Client for Frontend
This is a core feature. While a shared language (e.g. TypeScript) makes integration easier, tightly coupling the systems limits backend language choice. The solution is to reuse the backend’s existing type definitions to automatically generate a fully-typed client for the frontend.

For projects that need to support multiple external clients in different languages, OpenAPI works well as an intermediary (with good support from tools like TSOA, FastAPI, Huma, etc.). However, most REST frameworks still have significant gaps.

### Built-in Streaming Events (SSE)
Lack of first-class support for real-time events is a frequent pain point. These frameworks treat Server-Sent Events (SSE) as a first-class feature rather than an afterthought.

### First-Class Large File Upload Support
Many OpenAPI-centric frameworks handle small payloads well but struggle with large files. A seemingly normal 2 MB JSON upload requirement can quickly turn into an 800 MB edge case. These frameworks support large uploads natively, without awkward workarounds or conflicting middleware.

### Automatic Backend Change Detection & Client Regeneration
In true fullstack development, changes to the backend should immediately reflect in the frontend. The generated client is automatically regenerated on backend changes, and the browser refreshes automatically. This enables seamless context switching between backend (multithreading, databases, networking) and frontend work.

### Frontend HMR (Hot Module Replacement)
Fast frontend iteration is essential. These frameworks provide proper HMR during development, avoiding slow rebuilds and manual refreshes that kill developer flow.

### Decentralized Route/Handler Declaration
I organize code by concept, not by technical layer. I don’t want to constantly jump between a central server file and scattered implementations. Handlers should be declared where they naturally belong, and the framework should discover and wire them automatically. This significantly reduces cognitive load and keeps you in flow.

## Antipatterns (What not to do)

### SSR by Default
Server-Side Rendering has a high cost/benefit barrier. In my experience, only about 1 in 200 projects truly benefits enough from SSR to justify the added complexity over using CSR.

### Backend + Frontend Reload Instead of Proper HMR
A single “reload everything” approach is simpler to implement but delivers a noticeably worse developer experience. Combining backend auto-reload with frontend HMR provides the smoothest workflow.

### Frontend HMR Without Backend Auto-Reload
Forcing manual frontend refreshes after backend changes breaks flow. Both sides should stay in sync automatically.

### Manual Client Generation
Requiring a manual terminal command to regenerate the frontend client is unacceptable. It should happen transparently on backend changes.

### Lazy Dependencies (“Carry me, I’m too lazy to walk”)
Well-tested, actively maintained dependencies are valuable. However, not every problem should be solved with yet another package. I carefully weigh the benefits of a dependency against its maintenance status, replacement cost, and supply-chain risk. Prefer standard library solutions and battle-tested libraries when they truly add value.
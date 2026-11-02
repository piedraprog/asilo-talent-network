# Arquitectura de carpetas (Astro)

Adaptación del estándar Angular (`standards/01-arquitectura-folders.md`,
referencia `selltly_admin` / `ops-hub-frontend`) al router file-based de Astro.
Sin TanStack Router: las URLs siguen saliendo de `src/pages/`.

```
src/
  app/
    core/                         # lógica de negocio (loaders, validación, stores, modelos)
    pages/
      public/<feature>/           # UI sin sesión (home, auth, proyectos, recursos…)
      private/<feature>/          # UI con sesión (perfil, anuncios, onboarding…)
        components/               # piezas solo de esa feature
    shared/                       # 2+ features/dominios distintos
      components/
      layouts/
      scripts/
      styles/
  pages/                          # shells finos de Astro + `api/` (definen la URL)
```

## Reglas

- Algo usado por **una** feature vive en `app/pages/.../components/`.
- Pasa a `app/shared/` solo si lo usan **dos o más** features/dominios.
- Lógica de negocio y modelos van en `app/core/`, no inline en páginas.
- `src/pages/*.astro` solo importa y renderiza la page de `app/pages/...`
  (o el endpoint de `api/` llama a `app/core`).
- Navegación: file-based Astro. Auth/guards: middleware de Astro cuando haga falta.
  Transiciones opcionales: `astro:transitions` / `<ClientRouter />`.

# WebAPP - Playwright E2E Test Suite

Proyecto de pruebas End-to-End utilizando [Playwright](https://playwright.dev/), con soporte para múltiples navegadores y CI/CD con GitHub Actions.

---

## Tecnologías

- [Playwright](https://playwright.dev/) `v1.59`
- [TypeScript](https://www.typescriptlang.org/)
- [Node.js](https://nodejs.org/) `v18+`
- [GitHub Actions](https://github.com/features/actions)
- [ESLint](https://eslint.org/) + TypeScript ESLint

---

## Requisitos previos

- Node.js v18 o superior
- npm v8 o superior
- Docker (para pruebas E2E con BD staging)

---

## Instalación

```bash
# Clonar el repositorio
git clone https://github.com/elnegr0/webapp-playwright-e2e.git
cd webapp-playwright-e2e

# Instalar dependencias
npm install

# Instalar los navegadores de Playwright
npx playwright install
```

---

## Comandos disponibles

| Comando | Descripción |
|---|---|
| `npm test` | Ejecuta todos los tests en Chromium, Firefox y WebKit |
| `npm run test:ui` | Abre la interfaz visual interactiva de Playwright |
| `npm run test:report` | Abre el reporte HTML del último run |
| `npm run lint` | Analiza el código con ESLint |
| `npm run lint:fix` | Corrige errores de ESLint automáticamente |

---

## Estructura del proyecto

```
webapp-playwright-e2e/
├── .github/
│   └── workflows/
│       └── ci.yml              # Pipeline unificado de CI/CD
├── tests/
│   └── example.spec.ts         # Tests de ejemplo
├── eslint.config.js            # Configuración de ESLint
├── playwright.config.ts        # Configuración de Playwright
├── package.json
└── README.md
```

---

## Navegadores configurados

Los tests se ejecutan en paralelo sobre tres navegadores:

| Navegador | Engine |
|---|---|
| Chromium | Desktop Chrome |
| Firefox | Desktop Firefox |
| WebKit | Desktop Safari |

---

## Tests disponibles

### `tests/example.spec.ts`

| Test | Descripción |
|---|---|
| `has title` | Verifica que la página de Playwright tiene el título correcto |
| `get started link` | Verifica que el link "Get started" navega a la sección de instalación |

---

## Configuración de Playwright

| Opción | Valor |
|---|---|
| `testDir` | `./tests` |
| `fullyParallel` | `true` |
| `retries` | `2` en CI / `0` en local |
| `reporter` | HTML |
| `trace` | En el primer reintento |

---

## CI/CD

El pipeline unificado (`ci.yml`) se activa en cada `push` y `pull_request` a `main/master` con tres jobs:

```
push / pull_request
        │
        ▼
  ┌───────────┐
  │   lint    │  ← ESLint: valida calidad del código
  └─────┬─────┘
        │ si pasa ✅
        ├──────────────────┐
        ▼                  ▼
  ┌───────────┐     ┌───────────┐
  │   test    │     │    e2e    │  ← corren en paralelo
  └───────────┘     └───────────┘
```

| Job | Descripción |
|---|---|
| `lint` | Analiza el código con ESLint. Si falla, detiene el pipeline |
| `test` | Ejecuta los tests en Chromium, Firefox y WebKit |
| `e2e` | Ejecuta pruebas E2E en Chromium con MongoDB staging en Docker |

### Caché de browsers

Los browsers de Playwright se cachean por versión (`package-lock.json`), evitando reinstalarlos en cada push y reduciendo el tiempo de CI significativamente.

### MongoDB Staging en CI

El job `e2e` levanta automáticamente un contenedor de MongoDB con las credenciales de staging:

| Parámetro | Valor |
|---|---|
| Usuario | `test` |
| Contraseña | `test123` |
| Base de datos | `staging` |
| Puerto | `27018` |

---

## Ejecución local

```bash
# Correr todos los tests
npm test

# Correr solo en un navegador específico
npx playwright test --project=chromium
npx playwright test --project=firefox
npx playwright test --project=webkit

# Correr un archivo específico
npx playwright test tests/example.spec.ts

# Modo debug
npx playwright test --debug
```

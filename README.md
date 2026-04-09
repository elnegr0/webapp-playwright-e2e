# WebAPP - Playwright E2E Test Suite

Proyecto de pruebas End-to-End utilizando [Playwright](https://playwright.dev/), con soporte para múltiples navegadores y CI/CD con GitHub Actions.

---

## Tecnologías

- [Playwright](https://playwright.dev/) `v1.59`
- [TypeScript](https://www.typescriptlang.org/)
- [Node.js](https://nodejs.org/) `v18+`
- [GitHub Actions](https://github.com/features/actions)

---

## Requisitos previos

- Node.js v18 o superior
- npm v8 o superior

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

---

## Estructura del proyecto

```
webapp-playwright-e2e/
├── .github/
│   └── workflows/
│       └── playwright.yml      # Pipeline de CI/CD
├── tests/
│   └── example.spec.ts         # Tests de ejemplo
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

El pipeline de GitHub Actions se activa automáticamente con cada `push` que modifique archivos en:
- `src/**`
- `tests/**`
- `.github/workflows/playwright.yml`

### Pasos del pipeline

```
Checkout → Setup Node.js 18 → npm install → npm test
```

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

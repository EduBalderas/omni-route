# omni-route

## META
- id: omni-route
- owner: EduBalderas
- visibility: public
- idioma: en
- estado: estable
- tipo: producto
- nivel: L3
  - L0=gobernanza/política
  - L1=marca/metodología
  - L2=flujo/arquitectura
  - L3=producto ejecutable
  - L4=herramienta
- familia: ninguna
- origen: fork
- experimento: no
- nacido_de: diegosouzapw/OmniRoute
- orquestado_por: N/A
- brief_desde: N/A

## OBJETIVO
Gateway local autoalojado que expone un endpoint compatible con OpenAI y
enruta hacia 372 proveedores de LLM con fallback automático.

## QUE_CONTIENE
- `src/` — app Next.js 16 (dashboard y API), política, SQLite, A2A, memoria y skills.
- `open-sse/` — motor de streaming: handlers, executors, translators, combos y servidor MCP.
- `bin/` — CLI `omniroute` (serve, chat, doctor, configure y modo remoto).
- `electron/` — aplicación de escritorio para Windows, macOS y Linux.
- `src/lib/db/migrations/` — 202 migraciones SQL versionadas.
- `docs/` — guías, arquitectura, catálogo de proveedores y OpenAPI.
- `tests/` — node:test, Vitest, e2e de protocolos y ecosistema.
- `config/i18n.json` — locales de la UI; mirrors de documentación en `docs/i18n/`.

## CARACTERISTICAS
- Puerto por defecto 20128. Superficie `/v1` compatible con OpenAI, más formatos
  Claude, Gemini y Responses.
- 372 proveedores en el catálogo generado `docs/reference/PROVIDER_REFERENCE.md`.
  Las credenciales y las cuotas las aporta el operador.
- 20 estrategias de combo públicas en `src/shared/constants/routingStrategies.ts`:
  priority, weighted, round-robin, context-relay, fill-first, p2c, random,
  least-used, cost-optimized, reset-aware, reset-window, headroom,
  quota-weighted, strict-random, auto, lkgp, context-optimized, cache-optimized,
  fusion y pipeline. `quota-share` es interna y queda fuera de esa lista.
- Canal `auto` con variantes de peso en
  `open-sse/services/autoCombo/builtinCatalog.ts` (coding, fast, cheap,
  subscription, thrifty, offline, smart, chaos, entre otras) y `auto/lkgp` en
  `open-sse/services/autoCombo/autoPrefix.ts`.
- Resiliencia en tres capas: circuit breaker por proveedor, cooldown por
  conexión y lockout por modelo.
- Compresión apilable del payload elegible. El pipeline por defecto es RTK y
  después Caveman.
- MCP (stdio, SSE y HTTP), A2A JSON-RPC 2.0, memoria conversacional opt-in,
  guardrails y evals.
- Credenciales en reposo con AES-256-GCM. El plano de control corre en la
  máquina del operador y la telemetría sale desactivada.
- Distribución: paquete npm `omniroute`, imagen Docker, Electron, PWA y Termux.
  Licencia MIT.

## FUERA_DE_ALCANCE
- Los free tiers son cuentas de terceros. Este repositorio no hospeda modelos ni
  reparte cuota.
- La redacción de PII en la petición y en la respuesta permanece apagada salvo
  flag explícito (`PII_REDACTION_ENABLED`, `PII_RESPONSE_SANITIZATION`).
- El runtime publicado es Node `>=22.22.2 <23 || >=24.0.0 <27`. Bun no es
  runtime soportado.
- Las rutas que lanzan procesos (`/api/mcp/`, `/api/services/`,
  `/api/cli-tools/runtime/`) solo aceptan loopback.
- Los proveedores retirados a petición de su operador siguen fuera
  (`docs/reference/REMOVED_PROVIDERS.md`).
- La guía de instalación, los cupones y el material de sponsors viven en
  `docs/` y en el README del upstream.

## RELACIONES
- upstream: https://github.com/diegosouzapw/OmniRoute (autor y copyright:
  diegosouzapw)
- este fork: https://github.com/EduBalderas/omni-route
- npm: https://www.npmjs.com/package/omniroute
- Docker Hub: https://hub.docker.com/r/diegosouzapw/omniroute
- sitio: https://omniroute.online
- índice de documentación: `docs/README.md`
- extensión de editor, repositorio aparte:
  https://github.com/diegosouzapw/OmniCopilot
- bandeja de menú de la comunidad, fuera del release oficial:
  https://github.com/zoispag/omniroute-tray
- licencia: `LICENSE` (MIT)

## ETIQUETAS
- gateway-llm
- autoalojado
- compatible-openai
- fallback
- mcp

## TOPICS_GITHUB
- llm
- openai
- proxy
- mcp
- self-hosted

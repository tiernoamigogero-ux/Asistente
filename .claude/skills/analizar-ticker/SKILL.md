---
name: analizar-ticker
description: Ejecuta el equipo de bots de análisis financiero (negocio, resultados, filings SEC, fundamentales, management, técnico) sobre un ticker y devuelve un informe único consolidado. Úsalo cuando el usuario pida analizar una acción o dé un ticker para analizar.
---

# Analizar ticker

Recibe un ticker (ej. `AAPL`, `MELI`) y coordina al equipo de analistas para producir un informe único.

## Pasos

1. Si el usuario no dio un ticker, pídeselo antes de continuar.
2. Lanza en **un solo mensaje, en paralelo**, los siguientes subagentes con el Agent tool, pasándole a cada uno el ticker y pidiéndole que responda siguiendo su propio rol:
   - `business-analyst`
   - `earnings-analyst`
   - `filings-analyst`
   - `fundamentals-analyst`
   - `management-researcher`
   - `technical-analyst`
3. Cuando todos respondan, sintetiza sus hallazgos vos mismo (no delegues la síntesis) en **un informe único** con esta estructura:
   - **Resumen ejecutivo** (3-5 líneas: la tesis en una mirada).
   - **Negocio** (del business-analyst).
   - **Resultados recientes** (del earnings-analyst).
   - **Filings / riesgos regulatorios** (del filings-analyst).
   - **Fundamentales y valuación** (del fundamentals-analyst).
   - **Management e insiders** (del management-researcher).
   - **Técnico** (del technical-analyst).
   - **Señales de alerta** (consolidá los red flags que hayan mencionado varios analistas).
   - **Conclusión**: si el cuadro general luce favorable, mixto o desfavorable, y por qué — sin dar una recomendación de compra/venta.
4. Cerrá siempre el informe con esta aclaración textual: *"Este informe es información generada automáticamente con fines educativos, no es asesoramiento financiero ni una recomendación de inversión."*
5. Si algún analista no pudo obtener un dato, mantené esa limitación visible en el informe en vez de rellenarla.

## Notas

- Los subagentes usan búsqueda web pública (SEC EDGAR, cotizaciones, noticias) — no hay una conexión en vivo a un bróker. Si el usuario necesita datos de precio en tiempo real para trading, aclarale esa limitación.
- El equipo cubre 6 roles. Si en el futuro se agregan más bots especializados (ej. sentimiento, macro, valuación por DCF), agregá su agente en `.claude/agents/` y sumalo al paso 2.

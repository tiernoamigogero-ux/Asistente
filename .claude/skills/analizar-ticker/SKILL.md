---
name: analizar-ticker
description: Ejecuta el equipo completo de 10 bots de análisis financiero (negocio, resultados, filings SEC, fundamentales, management, técnico, sentimiento, calle, abogado del diablo y analista principal) sobre un ticker y devuelve un informe de iniciación único. Úsalo cuando el usuario pida analizar una acción o dé un ticker para analizar.
---

# Analizar ticker

Recibe un ticker (ej. `AAPL`, `MELI`, `CRDO`) y coordina al equipo de 10 analistas para producir un informe de iniciación único, replicando el flujo "chat grupal" del equipo.

## Pasos

1. Si el usuario no dio un ticker, pídeselo antes de continuar.
2. **Ronda 1 — investigación en paralelo.** Lanzá en **un solo mensaje, en paralelo**, los siguientes subagentes con el Agent tool, pasándole a cada uno el ticker:
   - `business-analyst`
   - `earnings-analyst`
   - `filings-analyst`
   - `fundamentals-analyst`
   - `management-researcher`
   - `technical-analyst`
   - `sentiment-analyst`
   - `street-analyst`
3. **Ronda 2 — caso bajista.** Una vez que los 8 anteriores respondieron, lanzá al subagente `devils-advocate`, pasándole en el prompt los hallazgos completos de los 8 analistas (pegalos tal cual, no los resumas) para que construya el caso bajista.
4. **Ronda 3 — informe final.** Lanzá al subagente `lead-analyst`, pasándole en el prompt los hallazgos completos de los 9 analistas anteriores (incluido el abogado del diablo). Pedile que devuelva el informe de iniciación completo con la estructura fija que tiene definida (Resumen Ejecutivo, Descripción del Negocio, Revisión de Ganancias, Balance General y Valoración, Evaluación de la Gerencia, Configuración Técnica, Sentimiento, Catalizadores y Visión de la Calle, Caso Bajista, Llamado Final) y el veredicto final (Comprar / Observar / Evitar, precio objetivo a 12 meses, y el precio o evento exacto que cambiaría la opinión).
5. Mostrale al usuario la respuesta del `lead-analyst` como informe final. No la reescribas ni la resumas vos: es el informe consolidado del equipo.

## Notas

- Los analistas de investigación (1 a 8) usan búsqueda web pública (SEC EDGAR, noticias, cotizaciones) — no hay conexión en vivo a un bróker ni acceso nativo a X/Twitter para el sentimiento. Si el usuario necesita datos de precio u opciones en tiempo real, aclarale esa limitación.
- `devils-advocate` y `lead-analyst` no hacen investigación propia: solo trabajan sobre lo que ya reportó el resto del equipo. Por eso es clave pasarles los hallazgos completos, no resúmenes armados por vos.
- El informe final siempre debe incluir la aclaración de que no es asesoramiento financiero.

# Asistente — Equipo de análisis financiero

Equipo de 10 sub-agentes de Claude Code que analizan una acción en paralelo, cada uno con un rol fijo, y consolidan todo en un informe de iniciación único al estilo de un fondo de cobertura. Inspirado en el patrón de "bot team" para análisis de acciones.

## Uso

En Claude Code, dentro de este repo:

```
/analizar-ticker AAPL
```

Esto corre las 3 rondas del equipo (investigación en paralelo → caso bajista → informe final) y devuelve el informe consolidado con veredicto.

## Equipo

| Agente | Rol |
|---|---|
| `business-analyst` | Qué hace la empresa, clientes, segmentos, competencia |
| `earnings-analyst` | Últimos 4 trimestres vs. consenso, guía, earnings call |
| `filings-analyst` | 10-K, 10-Q, 8-K en SEC EDGAR: riesgos, deuda, letra chica |
| `fundamentals-analyst` | Tabla de 5 años, múltiplos de valuación vs. competencia |
| `management-researcher` | CEO/CFO, insiders (Form 4), señales de alerta |
| `technical-analyst` | Tendencia de precio, soportes/resistencias, momentum |
| `sentiment-analyst` | Sentimiento reciente del mercado (vía búsqueda web, sin acceso nativo a X) |
| `street-analyst` | Cobertura de Wall Street, ratings, precios objetivo, catalizadores fechados |
| `devils-advocate` | Caso bajista más fuerte, atacando los hallazgos del resto del equipo |
| `lead-analyst` | Informe de iniciación final y veredicto (Comprar / Observar / Evitar) |

Cada agente vive en `.claude/agents/<nombre>.md`. El orquestador es el skill `.claude/skills/analizar-ticker/SKILL.md`, que corre las 3 rondas y devuelve el informe del `lead-analyst` como resultado final. `devils-advocate` y `lead-analyst` no investigan por su cuenta: solo juzgan lo que ya reportó el resto del equipo.

## Limitaciones

- Usa búsqueda web pública, no una conexión en vivo a un bróker (sin cotizaciones en tiempo real ni datos de opciones).
- El informe es información generada automáticamente con fines educativos, **no es asesoramiento financiero ni una recomendación de inversión**.

## Extender el equipo

Para agregar un analista nuevo (ej. sentimiento de mercado, valuación por DCF, contexto macro):

1. Creá `.claude/agents/<nombre>.md` con frontmatter `name`, `description` y `tools`, y el prompt de rol en el cuerpo.
2. Sumalo a la lista de agentes lanzados en el paso 2 de `.claude/skills/analizar-ticker/SKILL.md`.

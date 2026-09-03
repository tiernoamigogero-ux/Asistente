# Asistente — Equipo de análisis financiero

Equipo de sub-agentes de Claude Code que analizan una acción en paralelo, cada uno con un rol fijo, y consolidan todo en un informe único. Inspirado en el patrón de "bot team" para análisis de acciones.

## Uso

En Claude Code, dentro de este repo:

```
/analizar-ticker AAPL
```

Esto lanza en paralelo a los 6 analistas y devuelve un informe consolidado.

## Equipo

| Agente | Rol |
|---|---|
| `business-analyst` | Qué hace la empresa, clientes, segmentos, competencia |
| `earnings-analyst` | Últimos 4 trimestres vs. consenso, guía, earnings call |
| `filings-analyst` | 10-K, 10-Q, 8-K en SEC EDGAR: riesgos, deuda, letra chica |
| `fundamentals-analyst` | Tabla de 5 años, múltiplos de valuación vs. competencia |
| `management-researcher` | CEO/CFO, insiders (Form 4), señales de alerta |
| `technical-analyst` | Tendencia de precio, soportes/resistencias, momentum |

Cada agente vive en `.claude/agents/<nombre>.md`. El orquestador es el skill `.claude/skills/analizar-ticker/SKILL.md`, que sintetiza las 6 respuestas en un informe único con conclusión.

## Limitaciones

- Usa búsqueda web pública, no una conexión en vivo a un bróker (sin cotizaciones en tiempo real ni datos de opciones).
- El informe es información generada automáticamente con fines educativos, **no es asesoramiento financiero ni una recomendación de inversión**.

## Extender el equipo

Para agregar un analista nuevo (ej. sentimiento de mercado, valuación por DCF, contexto macro):

1. Creá `.claude/agents/<nombre>.md` con frontmatter `name`, `description` y `tools`, y el prompt de rol en el cuerpo.
2. Sumalo a la lista de agentes lanzados en el paso 2 de `.claude/skills/analizar-ticker/SKILL.md`.

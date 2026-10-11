# Checkpoint 1 · Agente base y motor de razonamiento (n8n)

**Alumna:** Glorianna Alfonzo Jones
**Proyecto integrador:** Asistente de Calificación de Leads de **Sowe**, empresa de consultoría de software y automatizaciones.

Archivo entregable: [`checkpoint1_glorianna_alfonzo_jones.json`](checkpoint1_glorianna_alfonzo_jones.json). Para usarlo, importalo en n8n con *Import from File*.

## Arquitectura del flujo

```
[Chat Trigger] ──► [AI Agent · Tools Agent · Max Iterations = 5] ──► [Gmail · Log de Observabilidad]
                         │                    │
                 (Chat Model)              (Tool)
          [Anthropic Chat Model]   [Google Sheets: Registrar Lead]
```

| Componente | Nodo | Configuración clave |
|---|---|---|
| Disparador | `Chat Trigger` | Captura el mensaje inicial del cliente potencial. |
| Cerebro | `AI Agent` | Modo **Tools Agent**. Guardrail: **Max Iterations = 5**. Devuelve los pasos intermedios. |
| Modelo | `Anthropic Chat Model` | Claude Sonnet, con temperatura 0.2 para respuestas más deterministas. |
| System Prompt | dentro del AI Agent | Modular: Rol → Ámbito → Objetivo → Proceso → Criterios → Uso de herramientas → Reglas y restricciones → Escalamiento → Estilo. |
| Herramienta | `Google Sheets Tool` (conectada al costado del agente) | Agrega el lead a la hoja `Leads`. Tiene una descripción semántica extensa de cuándo usarla y cuándo no. Los campos se completan con `$fromAI()`. |
| Observabilidad | `Gmail` (nodo final) | Envía por email el *Execution Log*: mensaje, respuesta, herramientas usadas, parámetros y resultado. |

## Criterios de calificación

El puntaje va de 0 a 100 y suma cuatro criterios:

| Criterio | Puntos |
|---|---|
| Necesidad | 0–30 |
| Presupuesto | 0–25 |
| Plazo | 0–25 |
| Autoridad | 0–20 |

Categorías según el puntaje:

| Categoría | Puntaje |
|---|---|
| Caliente | 70 o más |
| Tibio | 40–69 |
| Frío | menos de 40 |
| Escalar | Casos que requieren intervención humana |

## Hoja de cálculo

`plantilla_leads.csv` tiene los encabezados de la hoja `Leads`, que el flujo necesita para funcionar.

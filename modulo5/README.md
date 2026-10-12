# Módulo 5 · RAG: el cerebro documental del agente (Sowe)

Pre-entrega: [`PreEntrega_Modulo5_GloriannaAlfonzoJones.pdf`](PreEntrega_Modulo5_GloriannaAlfonzoJones.pdf)

| Archivo | Contenido |
|---|---|
| `Manual_Servicios_Politicas_Sowe_v1.pdf` | Documento maestro (fuente de verdad) |
| `Manual_Servicios_Politicas_Sowe_v1_curado.md` | Salida de LlamaParse curada a mano (títulos ##/###, tablas Markdown, §5.3 corregida). Es lo único que se indexa |
| `subworkflow_rag_m5.json` | Sub-workflow **publicado** "M5 - Sub-workflow RAG Sowe (buscar_manual_sowe)": Vector Store `sowe_manual_v1`, Top-K 4, Minimum Score 0.40, auto-ingesta si el índice está vacío y trigger de gobernanza para re-indexar |
| `M5_Sowe_RAG_workflow.json` | Workflow principal (M4 + herramienta `buscar_manual_sowe` + System Prompt con §10 RAG). Al importarlo, elegir el sub-workflow en la herramienta (From list) |
| `system_prompt_manager_m5.txt` | System Prompt del Manager (citas obligatorias + regla "No sé") |
| `evidencias/` | Resultados de la batería de 5 preguntas ciegas (iteración 2: 5/5) |
| `capturas/` | Capturas usadas en el PDF |

**Precisión:** iteración 1 = 0/5 (el modo JSON del modelo impedía llamar a la herramienta) → iteración 2 = 5/5 con la cita de sección.
**Gobernanza:** Líder de Operaciones; revisión mensual (catálogo y políticas comerciales) y trimestral (garantía, SLA, FAQ); re-indexado con Clear Store + 5 preguntas ciegas después de cada cambio.

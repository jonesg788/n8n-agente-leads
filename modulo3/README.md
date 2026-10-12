# Módulo 3 · Memoria persistente y resumen automático (Sowe)

Este módulo parte del Manager de M2 (patrón Manager-Worker) y le suma una capa de memoria híbrida:

- **Corto plazo:** Simple Memory, con clave `Session_ID`.
- **Largo plazo:** la pestaña `Memoria_Sesiones` de Google Sheets.

## Archivos

| Archivo | Descripción |
|---|---|
| `PreEntrega_Modulo3_GloriannaAlfonzoJones.pdf` | Documento técnico de la pre-entrega: lienzo, prompt del modelo mini, esquema de datos y evidencia. |
| `manager_modulo3_alfonzo_jones_glorianna.json` | Workflow "M3 - Manager Sowe (Memoria persistente)". |
| `plantilla_memoria_sesiones.csv` | Encabezados de la pestaña `Memoria_Sesiones`. |
| `prompt_summarization.txt` | Prompt de gpt-4o-mini que genera el JSON `{asunto_principal, puntos_clave, accion_requerida}`. |
| `system_prompt_manager_m3.txt` | System Prompt del Manager. Incluye el bloque `[INICIO DE CONTEXTO COMPARTIDO]` … `[FIN DEL CONTEXTO COMPARTIDO]`. |

Los Workers son los mismos de M2: ver [`../modulo2/`](../modulo2/).

## Flujo

1. **Lectura:** el Chat Trigger dispara una búsqueda en `Memoria_Sesiones` por `Session_ID`.
2. **IF de usuario:**
   - Si el usuario es nuevo, se crea su fila inicial.
   - Si ya escribió antes, se arma su contexto: nombre, estado, resumen y datos clave.
3. **Agente:** el AI Agent (Tools Agent, gpt-4o en modo JSON) recibe la memoria y el contexto inyectado entre delimitadores.
4. **Validación:** un Set valida el JSON del agente.
5. **Orquestación de M2:** Router, Workers W1 y W2, Log y alerta humana.
6. **Persistencia:** se guarda el estado con *Append or Update* por `Session_ID`.
7. **Summarization:** si la sesión supera 5 mensajes, Chat Memory Manager pasa el historial a gpt-4o-mini, que genera el JSON. Ese resumen se guarda de forma idempotente en `Resumen_Consolidado`.

## Cómo importarlo

1. Importá el JSON en n8n con *Import from File*.
2. Asigná las credenciales de Google Sheets, OpenAI y Gmail.
3. Vinculá los Workers de M2 en los nodos "Delegar a W1" y "Delegar a W2".
4. Revisá que los filtros y las columnas de coincidencia usen `Session_ID`.

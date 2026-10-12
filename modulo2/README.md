# Módulo 2 · Orquestación Multi-Agente (Manager-Worker)

Evolución del agente de M1: la lógica de calificación y la tool de Google Sheets pasan a sub-workflows especializados.

| Archivo | Contenido |
|---|---|
| `manager_modulo2_alfonzo_jones_glorianna.json` | Orquestador: Chat Trigger → AI Agent de triaje (gpt-4o, taxonomía cerrada) → Switch → Set + Execute Workflow (Wait for child to finish) → Log en Sheets → Gmail de escalamiento |
| `worker1_modulo2_alfonzo_jones_glorianna.json` | Worker 1: valida los datos, califica el lead (gpt-4o-mini) y lo registra en la hoja `Leads` |
| `worker2_modulo2_alfonzo_jones_glorianna.json` | Worker 2: redacta el borrador del email de seguimiento (gpt-4o-mini) |
| `plantilla_log_orquestacion.csv` | Encabezados de la pestaña `Log_Orquestacion` |
| `preentrega_modulo2_alfonzo_jones_glorianna.pdf` | Entregable: capturas, esquema de datos y criterio de enrutamiento |

## Para importar
1. Importá primero los dos Workers y después el Manager.
2. En el Manager, en los nodos **Delegar a W1** y **Delegar a W2**, elegí el Worker correspondiente.
3. Cargá las credenciales (OpenAI, Google Sheets y Gmail) y la URL de la hoja.

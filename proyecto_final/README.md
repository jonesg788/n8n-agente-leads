# Proyecto Final Integrador · Asistente Inteligente Sowe

DMSA: [`AlfonzoJones_Glorianna_ProyectoFinal_AI_Experto.pdf`](AlfonzoJones_Glorianna_ProyectoFinal_AI_Experto.pdf)

| Workflow | Rol |
|---|---|
| `PF_Manager_Sowe.json` | Orquestador: Webhook POST `sowe-consultas` → 202 inmediato → idempotencia y memoria (Sheets `Sistema_Final`) → agente RAG gpt-4o-mini → Supervisor → Switch ACEPTADO / CORREGIR (máx. 2) / RECHAZADO → HITL en Slack · ramas de error con alerta |
| `PF_Worker_Despacho.json` | Worker: HubSpot create-or-update por email + Slack #sowe-operaciones |
| `../modulo5/subworkflow_rag_m5.json` | Worker RAG (manual v1.0, Top-K 4, Min Score 0,40) |
| `../modulo8/subworkflow_supervisor_judge_m8.json` | Supervisor AI-as-a-Judge (gpt-4o, JSON Schema) |

Contrato de entrada: `{request_id*, mensaje*, nombre, empresa, email, canal}`. Las credenciales se referencian por nombre en n8n; no hay API keys en este repositorio.

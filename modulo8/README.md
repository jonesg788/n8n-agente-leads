# Módulo 8 · QA automatizado con AI-as-a-Judge

Entregable: [`Checkpoint_Modulo8_AlfonzoJones_Glorianna.pdf`](Checkpoint_Modulo8_AlfonzoJones_Glorianna.pdf)

- `subworkflow_supervisor_judge_m8.json`: SUPERVISOR_Judge (gpt-4o, temperatura 0, Structured Output Parser con JSON Schema, umbral determinista y regla de consistencia). Se publica.
- `M8_QA_AI_as_Judge_AB.json`: agente RAG de Sowe + juez + Switch de 3 rutas (ACEPTADO → Slack; CORREGIR → self-healing con máximo 2 reintentos; RECHAZADO → Slack Send and Wait con Aprobar / Editar), log en Sheets `QA_Veredictos` y dashboard A/B. El nodo Golden Dataset tiene dos modos: `MODO = 'AB'` para las 10 corridas y `MODO = 'HITL'` para la inyección de falla.
- `rubrica_juez_m8.txt`: prompt y JSON Schema del juez.
- `evidencias/`: CSV de QA_Veredictos de la iteración 1 (ejecución 92), la iteración 2 (114) y la prueba HITL (139).

Resultado: A (gpt-4o) y B (gpt-4o-mini) llegan al 100% de precisión final. B lo logra al primer intento y cuesta unas 2,6 veces menos (US$ 7,21 contra US$ 18,82 cada 1.000 ejecuciones) → se recomienda B como ejecutor y gpt-4o como juez.

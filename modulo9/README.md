# Módulo 9 · Plan de gobernanza, gestión y monitoreo financiero de IA

Pre-entrega: [`PreEntrega_Modulo9_GloriannaAlfonzoJones.pdf`](PreEntrega_Modulo9_GloriannaAlfonzoJones.pdf)

1. **Finanzas:** KPIs para la dirección; ROI contra el baseline manual (95,5 h/mes ahorradas ≈ US$ 1.910/mes); TCO de ≈ US$ 156/mes (≈ US$ 152 con evaluación selectiva del juez); ROI del 1.121% y payback de 0,9 meses; prompt caching de Anthropic y de OpenAI; llaves DEV/PROD con límites de US$ 5/mes.
2. **Trazabilidad:** columna `traceId = sowe-{execution.id}-{ID_Corrida}` sobre QA_Veredictos (M8), recorrido del traceId por los sub-workflows y esquema de log JSON (timestamp, nodeName, toolOutput, systemPromptSnapshot, confidenceScore).
3. **Ética y compliance:** 5 riesgos con su control y su ubicación en el flujo, bloque ético del System Prompt y privacidad por proveedor (OpenAI, Anthropic, ElevenLabs).

Supuestos: 1.000 ejecuciones/mes y hora hombre de US$ 20 (a validar con la empresa).

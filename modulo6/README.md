# Módulo 6 · Circuito cerrado de voz (Voice AI) · Sowe

Pre-entrega: [`PreEntrega_Modulo6_GloriannaAlfonzoJones.pdf`](PreEntrega_Modulo6_GloriannaAlfonzoJones.pdf)

`M6_Sowe_VoiceAI_workflow.json` es el workflow del M5 (chat + email + CRM + Slack + RAG) más la capa de voz:

Telegram Trigger → Telegram Get a file (binario `data`) → Whisper (`data`, `es`, temperatura 0) → IF de contingencia (texto de 3 caracteres o más y sin error)
→ Agente de Voz (Tools Agent, gpt-4o-mini, RAG `buscar_manual_sowe_voz`, regla de 200 caracteres como máximo) → contención determinista de 200 caracteres
→ ElevenLabs (Bella, eleven_multilingual_v2, stability 0.6, similarity_boost 0.8) → Telegram Send Audio → Compliance: destrucción de binarios.

**Compliance:** no se guarda ninguna ejecución (ni exitosa, ni fallida, ni manual) y ningún nodo de voz escribe en un almacén persistente.

**Al importar:** asignar las credenciales de Telegram, OpenAI y ElevenLabs, elegir el sub-workflow RAG del M5 en `buscar_manual_sowe` y en `buscar_manual_sowe_voz`, y revisar el campo `consulta` (`$fromAI`). No publicar el workflow, porque incluye el Gmail Trigger del M4. Para probar: Execute workflow → Telegram Trigger.

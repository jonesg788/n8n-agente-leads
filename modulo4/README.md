# Módulo 4 · Integraciones avanzadas (Gmail + HubSpot + Slack)

**Archivo entregable:** [`../checkpoint4_glorianna_alfonzo_jones.json`](../checkpoint4_glorianna_alfonzo_jones.json). Es el export de n8n del workflow "M4 - Sowe Integraciones (Chat + Email + CRM + Slack)".

El workflow parte del M3 (chat con memoria persistente y Manager-Worker) y le agrega un **canal de email** integrado con tres herramientas externas, todas conectadas por **OAuth2**:

| Herramienta | Rol | Lectura (pasado) | Escritura (futuro) |
|---|---|---|---|
| **Gmail** | Casilla de soporte | Gmail Trigger: correos no leídos del INBOX | Solo **Create Draft** (borrador) |
| **HubSpot** | CRM | Look up de contacto por email | Create / Update de contacto (email, nombre, empresa, etapa *lead*) |
| **Slack** | Canal de operaciones | — | Mensaje en `#sowe-operaciones` |

## Flujo del canal de email

```
Gmail Trigger → ① IF ¿auto-reply? ──Sí──► Stop (corta el bucle)
                     │ No
                     ▼
     Agente de Soporte (AI Agent · gpt-4o-mini · JSON) → Interpretar salida
                     ▼
     ④ Set limpiar y validar payload → ¿Email válido? ─No─► Slack
                     ▼ Sí
     ② Look up HubSpot → ¿Existe? ─Sí─► Update │ No─► Create
                     ▼
     ③ Create Draft (Gmail, HITL) → Set payload mínimo → Slack
```

## Los 4 controles de la rúbrica

1. **① IF anti auto-reply:** el workflow ignora un correo si se cumple cualquiera de estas condiciones (OR):
   - el asunto contiene *Auto-reply*, *Out of office*, *Undeliverable* u otra marca de respuesta automática;
   - el remitente es *no-reply*, *mailer-daemon* o similar;
   - trae el encabezado `Auto-Submitted`;
   - es una alerta `[Sowe | …]` del propio n8n. Esto evita el bucle, porque la casilla de soporte es la misma que envía las alertas.
2. **② Look up antes del Create:** busca el contacto por email. Si existe, actualiza; si no, crea. Así se evita el Error 409 por duplicados.
3. **③ Create Draft:** la IA nunca envía el correo. Deja un borrador en Gmail para aprobación humana (Human-in-the-loop).
4. **④ Set de limpieza:** se queda solo con remitente, asunto, texto plano (máximo 2.000 caracteres) e ID de hilo, sin HTML ni adjuntos. Además valida el formato del email, lo que evita el Error 400. El aviso a Slack pasa por otro Set mínimo que arma un único campo `text`.

## Mínimo privilegio

- **HubSpot:** solo opera sobre contactos y solo con las propiedades necesarias.
- **Gmail:** lee la bandeja y crea borradores, nunca envía.
- **Slack:** solo publica en un canal.

## Test de regresión manual

Las pruebas se hicieron con *Execute step* sobre correos reales. Las capturas están en [`capturas/`](capturas/).

| Prueba | Resultado |
|---|---|
| Correo nuevo | El IF sale por *false*. Se crea el contacto en HubSpot, se genera el borrador en Gmail y llega el aviso a Slack. |
| Mismo remitente | El Look up lo encuentra y va por **Update**, sin duplicar el contacto. |
| Asunto "Auto-reply: Out of office" | El IF sale por *true* y va a **Stop**. No se ejecuta la IA. |

## Cómo importarlo

1. En n8n, usá *Import from File* con `checkpoint4_glorianna_alfonzo_jones.json`.
2. Reasigná las credenciales: Google Sheets, OpenAI, Gmail, HubSpot y Slack. Todas son OAuth2, salvo OpenAI.
3. Vinculá los Workers de M2 (carpeta [`../modulo2/`](../modulo2/)) en los nodos "Delegar a W1" y "Delegar a W2".

# Kapso + Aules · asistente personal por WhatsApp

Cron externo (GitHub Actions) para el proyecto Kapso `Whatsapp`.

## Qué hace

| Workflow | Frecuencia | Función Kapso |
| --- | --- | --- |
| `sync.yml` | cada 15 min | `aules_sync` — login en Aules y guarda las entregas en KV |
| `reminders.yml` | cada 5 min | `aules_reminders` — envía los avisos que toquen por WhatsApp |

Los recordatorios se calculan en hora de Madrid:

- día anterior a las 09:00
- el mismo día a las 18:00
- 1 h 30 min antes del cierre

Cada aviso se marca como enviado en KV (`sent:<mod_id>:<regla>`), así que no se duplica aunque el cron se ejecute varias veces.

## Secreto necesario

En **Settings → Secrets and variables → Actions → New repository secret**:

- `KAPSO_CRON_TOKEN` — token compartido que valida las funciones de Kapso (no está en el código).

## Notas

- Los endpoints de las funciones son públicos, pero rechazan peticiones sin `_token` (HTTP 401).
- El repo puede ser público: no contiene credenciales, solo el token en Secrets.
- Si GitHub retrasa el cron, la ventana de 15 min evita perder el aviso.
- El envío por WhatsApp desde el número **sandbox** solo funciona dentro de la ventana de 24 h desde tu último mensaje. Para avisos garantizados hace falta un número de producción + plantilla aprobada.

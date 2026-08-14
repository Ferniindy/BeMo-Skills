---
name: Resumen Diario (Plantilla Comercial)
description: Skill de orquestación matutina que actúa como un Asistente de Integración la primera vez para mapear las herramientas del gestor, y luego como un resumen dinámico automatizado.
---

# SKILL: Resumen Diario (Integration Wizard)

## Objetivo
Esta SKILL es la automatización estrella ("Momento Aha") para un Gestor de Centro Deportivo. Su objetivo es leer las prioridades del día, la agenda del centro y las alertas de clientes, presentándolas en un informe útil y amigable (Tono BeMo).

## Triggers
- "Dame mi resumen del día"
- "Buenos días"
- "Qué tenemos para hoy"
- O si es lanzada inmediatamente tras el Onboarding.

## Funcionamiento Multi-Plataforma (El Asistente de Integración)

Al ejecutarse, lo primero que debes hacer es comprobar si ya existe un archivo de configuración llamado `resumen_config.json` en tu carpeta de habilidades.

### FASE 1: Mapeo Inicial (Si no existe `resumen_config.json`)
Actúa como un Consultor Tecnológico. Explícale al usuario que para que el resumen sea útil, necesitas saber dónde guarda sus cosas.
Usa la herramienta `ask_question` para hacer las siguientes preguntas, DE UNA EN UNA:

1. **Agenda Operativa:** "¿Dónde llevas las clases de tu gimnasio y la agenda del centro? (Idealmente Google Calendar, o dime si usas un CRM)."
2. **Gestión de Leads/Pruebas:** "¿Dónde anotas los datos de las personas que vienen a probar por primera vez? (Notion, Google Calendar, Excel local, Google Sheets o CRM)."
3. **Tareas y Prioridades:** "¿Dónde apuntas tus tareas administrativas urgentes? (Notion, Trello, Google Tasks)."

**Importante sobre CRMs:** Si el usuario menciona que usa un CRM cerrado (Aimharder, Wodbuster, Timp), indícale amablemente que necesitarás acceso a su API Oficial para sacar los datos. Si no tienen API, puedes proponer usar Playwright (Web Scraping) como plan B, pero requerirá credenciales.

Una vez recopiladas las 3 respuestas, pide las claves, tokens, API Keys o URLs necesarias usando el **Bucle de Iteración Conjunta (Pair-Testing)** y la regla de **Zero Fake Executions**. *¡No finjas tener acceso si no lo tienes!*

Cuando tengas éxito leyendo de sus fuentes por primera vez, guarda las preferencias en `resumen_config.json` y el código generado en `resumen_usuario.js`.

### FASE 2: Ejecución Diaria (Si ya existe la configuración)
Si ya has mapeado sus integraciones:
1. Ejecuta el script `resumen_usuario.js` o extrae los datos usando tus herramientas nativas de conexión basándote en su `resumen_config.json`.
2. Saluda al usuario de forma enérgica (estilo BeMo).
3. Estructura el día en 3 bloques claros:
   - **📅 Agenda del Box:** (Volumen de clases y alertas de saturación).
   - **📞 Alertas de Clientes (Leads/Retención):** (Pruebas del día, cumpleaños, bajas).
   - **🎯 Tareas Administrativas:** (Prioridades altas para el gerente).
4. **Graceful Degradation:** Si por algún motivo falla una de las conexiones (ej. caducó el token de Notion), NO muestres un error técnico. Simplemente di: *"No he podido acceder a tus tareas de Notion hoy (parece que la conexión caducó), pero aquí tienes el resto del resumen..."* y ofrécete a arreglarlo.

Análisis de eventos de seguridad de Windows
Descripción

Este proyecto consiste en el análisis básico de eventos de seguridad registrados en Windows mediante el Visor de eventos, con el objetivo de identificar y documentar actividades relacionadas con los inicios de sesión.

El trabajo está orientado a desarrollar habilidades iniciales de análisis de eventos, investigación de posibles incidentes y documentación técnica, aplicables a tareas de un analista SOC Junior.

Objetivos

- Identificar eventos de inicio de sesión exitosos y fallidos.
- Interpretar información relevante de los registros de seguridad.
- Diferenciar actividad aparentemente normal de eventos que requieren una revisión adicional.
- Documentar hallazgos y proponer medidas preventivas.

Herramientas utilizadas

- Windows Event Viewer (Visor de eventos).
- Registros de Windows → Seguridad.
- Filtros por identificador de evento.
- Microsoft Word para la documentación del análisis.

Eventos analizados

Evento 4624 — Inicio de sesión correcto

Se analizó un evento de inicio de sesión exitoso asociado a una cuenta de sistema y a un proceso de servicios de Windows.

El tipo de inicio de sesión observado fue 5, correspondiente a un inicio de sesión de servicio. Con los datos disponibles, el registro resulta compatible con la actividad habitual de un servicio de Windows y no permite, por sí solo, identificar actividad maliciosa.

Evento 4625 — Error de inicio de sesión

Se analizó un evento de inicio de sesión fallido. Se revisaron el tipo de inicio de sesión, el motivo indicado, el proceso asociado y los códigos de estado registrados.

El evento requiere interpretarse en su contexto. Un único registro fallido no es suficiente para confirmar un ataque, y el proceso indicado en el evento no demuestra por sí solo que dicho programa haya causado la actividad.

Evidencias

La carpeta `evidencias/` contiene capturas de pantalla de los eventos analizados en el Visor de eventos de Windows.

Conclusiones

El análisis permitió practicar la búsqueda, el filtrado y la interpretación inicial de eventos de seguridad de Windows.

También permitió reconocer la importancia de documentar los hallazgos con evidencia verificable y evitar conclusiones que no estén respaldadas por los registros disponibles.

Alcance y limitaciones

Este es un ejercicio práctico de nivel inicial basado en eventos del propio sistema Windows. El análisis se limita a los registros examinados y no constituye una auditoría completa ni permite determinar por sí solo si el equipo está comprometido.

Próximas mejoras

- Analizar un conjunto más amplio de eventos de seguridad.
- Investigar patrones de múltiples intentos fallidos de inicio de sesión.
- Practicar la correlación de eventos y la elaboración de una línea temporal.
- Explorar herramientas de análisis y monitorización utilizadas en entornos SOC.


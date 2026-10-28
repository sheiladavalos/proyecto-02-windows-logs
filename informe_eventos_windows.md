# Análisis de eventos de seguridad de Windows

**Área:** SOC / Blue Team  
**Enfoque:** Análisis inicial de registros de autenticación  
**Herramienta:** Visor de eventos de Windows  
**Estado:** En desarrollo

## 1. Objetivo

El objetivo de esta práctica es analizar registros de seguridad de Windows para identificar inicios de sesión exitosos y fallidos, interpretar los campos relevantes y evaluar si existen indicios que justifiquen una investigación adicional.

Esta actividad reproduce una tarea habitual de un analista SOC de nivel inicial: revisar eventos, reconocer comportamientos normales, identificar anomalías y documentar conclusiones sin afirmar más de lo que demuestra la evidencia.

## 2. Metodología

Se utilizó el Visor de eventos de Windows, siguiendo esta ruta:

**Visor de eventos → Registros de Windows → Seguridad**

Se filtraron los eventos por los identificadores 4624 y 4625.

- **Evento 4624:** indica que se registró un inicio de sesión exitoso.
- **Evento 4625:** indica que se registró un intento de inicio de sesión fallido.

Se revisaron la fecha y hora, el tipo de inicio de sesión, la cuenta asociada, el proceso relacionado, la dirección de origen y los códigos de autenticación disponibles.

El análisis fue de solo lectura: no se modificó la configuración del equipo ni se generaron intentos de acceso deliberados.

## 3. Hallazgo 1: inicio de sesión exitoso

**Identificador:** 4624

**Fecha y hora:** 23/10/2026, 17:34:58.

**Datos observados:**

- Tipo de inicio de sesión: 5, correspondiente a un servicio.
- Cuenta: `SYSTEM`.
- Proceso: `C:\Windows\System32\services.exe`.

**Análisis:**

El tipo de inicio de sesión 5 se relaciona con el inicio de sesión de una cuenta para ejecutar un servicio. La cuenta `SYSTEM` y el proceso `services.exe` son compatibles con el funcionamiento habitual de Windows.

Con los datos disponibles, no se identificaron indicios suficientes para considerar sospechoso este evento. Para una validación más completa sería necesario revisar el contexto y los demás campos del registro.

## 4. Hallazgo 2: inicio de sesión fallido

**Identificador:** 4625

**Fecha y hora:** 23/10/2026, 18:30:57.

**Datos observados:**

- Tipo de inicio de sesión: 2, correspondiente a un inicio interactivo local.
- Motivo mostrado: nombre de usuario desconocido o contraseña incorrecta.
- Cuenta: reconocida como una cuenta conocida del equipo.
- Proceso relacionado: `C:\Program Files\Google\Chrome\Application\chrome.exe`.
- Paquete de autenticación: `Negotiate`.
- Dirección de red de origen: `-`.
- Código de estado: `0xC000006E`.
- Código de subestado: `0xC000006E`.

**Análisis:**

El evento indica que un intento de inicio de sesión no tuvo éxito. El código `0xC000006E` corresponde a una restricción de cuenta, por lo que es necesario considerar ese dato además del mensaje general que muestra Windows.

El valor `Negotiate` identifica un mecanismo de negociación de autenticación. La aparición de `chrome.exe` en el campo de proceso relacionado no demuestra, por sí sola, que Chrome haya originado un ataque ni permite establecer el mecanismo exacto del intento.

La dirección de origen aparece como `-`, por lo que ese campo no proporciona una dirección IP para este evento.

Durante la revisión se encontró un evento 4625 en el resultado consultado. No se confirmó que existieran intentos repetidos en una secuencia que permitiera sospechar de fuerza bruta. La usuaria no recuerda haber realizado el intento, pero esa circunstancia tampoco demuestra actividad maliciosa.

**Evaluación preliminar:** evento de causa no confirmada que merece conservarse documentado. Los datos actuales no permiten confirmar un ataque ni determinar con certeza por qué falló la autenticación.

## 5. Evaluación general

Los dos eventos muestran situaciones diferentes: un inicio de sesión exitoso relacionado con un servicio de Windows y un intento de autenticación fallido.

El primer evento es compatible con una actividad habitual del sistema. El segundo requiere más contexto antes de poder atribuirle una causa concreta.

La ausencia de una dirección IP de origen y la existencia de un único evento fallido en la búsqueda realizada limitan las conclusiones posibles. Esto no demuestra que el equipo esté libre de amenazas; únicamente describe lo observado en esta revisión.

## 6. Recomendaciones

1. Revisar si existen otros eventos 4625 en un intervalo temporal más amplio.
2. Correlacionar los eventos con registros cercanos en fecha y hora.
3. Examinar los detalles adicionales de autenticación y de la cuenta para comprender la restricción registrada.
4. Comprobar si existen otros indicadores, como múltiples fallos consecutivos o actividad inesperada.
5. Registrar las evidencias y mantener separadas las observaciones, las hipótesis y las conclusiones.
6. Si aparecen nuevos indicios de acceso no autorizado, ampliar la investigación antes de tomar medidas correctivas.

## 7. Conclusión

La práctica permitió utilizar el Visor de eventos de Windows, filtrar registros de seguridad y analizar los eventos 4624 y 4625. También permitió reconocer la importancia del tipo de inicio de sesión, la cuenta asociada, el proceso relacionado y los códigos de autenticación.

El resultado principal es un análisis preliminar con un evento exitoso compatible con el funcionamiento de Windows y un evento fallido cuya causa exacta no está confirmada. No existe evidencia suficiente, a partir de los datos examinados, para clasificar este último como un ataque.

Como siguiente etapa, se propone ampliar la revisión temporal y correlacionar los registros para determinar si aparece algún patrón relevante.

---

**Nota de alcance:** este informe documenta una práctica inicial sobre registros del equipo. No constituye una auditoría integral ni una certificación de que el sistema esté libre de incidentes.

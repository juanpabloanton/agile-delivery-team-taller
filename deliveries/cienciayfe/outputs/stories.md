# Historias de usuario refinadas — Ciencia y Fe

Fecha: 2026-06-23
Delivery: cienciayfe
Responsable: Developer (refinamiento INVEST + DoR)

---

## Épica E-01 · Esquema de datos centralizado de calificaciones

### US-06 · Esquema de BD centralizado de calificaciones   ·   épica E-01   ·   5 pts

**Como** desarrollador, **quiero** un esquema de base de datos centralizado con una tabla unificada de calificaciones que sirva a todos los cursos y reportes, **para** poder escribir una sola lógica de consulta reutilizable y eliminar los stored procedures duplicados por curso.

Criterios de aceptación (Gherkin):

- Dado que se consultan calificaciones de cualquier curso del período 2025-2026, cuando se ejecuta la consulta, entonces proviene de la misma tabla o vista unificada y no de tablas independientes por curso.
- Dado que se agrega un nuevo tipo de reporte, cuando se implementa, entonces reutiliza la lógica de consulta existente sin crear un nuevo stored procedure.
- Dado que existe el esquema centralizado, cuando el desarrollador revisa la base de datos, entonces no existen tablas de calificaciones duplicadas ni fragmentadas por curso.
- Dado que el esquema centralizado está diseñado, cuando el desarrollador lo valida antes de migrar, entonces existe una copia local de la base de datos de producción usada como entorno de validación (Supuesto SA-01: no existe ambiente de staging formal; la validación se realiza sobre copia local antes de aplicar en producción).

Dependencias: ninguna.
Origen: `us:US-06`, `req:R-04`, `pain:informacion-no-centralizada`, `pain:sp-no-mantenibles`

---

### US-06a · Migración de calificaciones al esquema centralizado   ·   épica E-01   ·   5 pts

**Como** desarrollador, **quiero** migrar los datos de calificaciones del período 2025-2026 del esquema fragmentado actual al nuevo esquema centralizado sin pérdida de información, **para** que los reportes del período 2025-2026 puedan generarse desde la fuente unificada sin depender de las tablas antiguas.

Criterios de aceptación (Gherkin):

- Dado que existen calificaciones en el esquema fragmentado anterior, cuando se ejecuta la migración, entonces el 100 % de los registros del período 2025-2026 están presentes en el esquema centralizado.
- Dado que se completa la migración, cuando se comparan los totales por curso entre el esquema antiguo y el nuevo, entonces los recuentos coinciden sin diferencia.
- Dado que la migración se ejecuta, cuando se revisa el log, entonces no se registran errores de integridad referencial.
- Dado que existen posibles inconsistencias en el esquema fragmentado, cuando el desarrollador ejecuta el script de validación previa a la migración, entonces obtiene un reporte de registros con datos vacíos o inconsistentes que él mismo revisa y limpia antes de correr la migración (Supuesto SA-02: la validación técnica la hace el desarrollador; la confirmación de una muestra de calificaciones del período 2025-2026 la realiza la secretaria antes de dar el visto bueno final).

Dependencias: US-06.
Origen: `mvp:riesgos·1`, `req:R-04`, `us:US-06`, `pain:informacion-no-centralizada`

---

## Épica E-02 · Generación de reportes consistentes y sin devoluciones

### US-01 · Nombres de materias dinámicos en cuadros de calificaciones   ·   épica E-02   ·   3 pts

**Como** secretaria, **quiero** que los cuadros de calificaciones muestren los nombres de las materias leídos dinámicamente desde la base de datos, **para** no tener que detectar ni corregir inconsistencias de nombres manualmente antes de enviar los reportes al distrito.

Criterios de aceptación (Gherkin):

- Dado que se genera un cuadro trimestral, cuando el sistema lo produce, entonces los nombres de las materias coinciden exactamente con los registrados en la base de datos, sin valores hardcoded en la plantilla.
- Dado que un nombre de materia cambia en la base de datos, cuando se regenera el reporte, entonces el cambio se refleja sin modificar la plantilla.
- Dado que se generan cuadros de distintos cursos, cuando se comparan los nombres de materias entre ellos, entonces provienen de la misma fuente de datos y no existen variantes textuales para la misma materia.

Dependencias: US-06.
Origen: `us:US-01`, `req:R-01`, `pain:nombres-materias-quemados`

---

### US-02 · Aislamiento de cambios entre reportes del mismo período   ·   épica E-02   ·   3 pts

**Como** secretaria, **quiero** que corregir el layout o los datos de un reporte no altere los demás reportes del mismo período, **para** poder revisar y aprobar cada documento de forma independiente sin miedo a romper otros reportes.

Criterios de aceptación (Gherkin):

- Dado que se corrige y regenera el cuadro trimestral, cuando se visualizan el reporte de promoción y el cuadro final, entonces su layout y datos no han cambiado.
- Dado que se actualiza el cuadro final, cuando se comparan con el reporte de promoción, entonces los nombres de materias y valores numéricos coinciden en ambos documentos.
- Dado que se modifica la configuración de un curso específico, cuando se regeneran los reportes de otros cursos, entonces no presentan cambios no intencionados.

Dependencias: US-01, US-05.
Origen: `us:US-02`, `req:R-02`, `pain:cambio-dana-otro-reporte`

---

### US-07 · Disponibilidad de todos los reportes 2025-2026 antes del cierre   ·   épica E-02   ·   2 pts

**Como** rectora, **quiero** que los cuadros trimestrales, finales y reportes de promoción del período 2025-2026 estén disponibles y correctos antes del cierre de período, **para** poder cumplir los compromisos de entrega de documentos académicos con el distrito a tiempo.

Criterios de aceptación (Gherkin):

- Dado que es el cierre del período 2025-2026, cuando la secretaria revisa los reportes generados, entonces los aprueba en el primer ciclo de revisión sin devoluciones por inconsistencias de nombres o layout roto.
- Dado que los reportes de segundo de básica a bachillerato son los críticos para el distrito, cuando se generan, entonces están disponibles y aprobados antes que los de inicial y básica inferior.
- Dado que se genera cualquier reporte del período 2025-2026, cuando la secretaria lo imprime, entonces no requiere edición manual para enviarlo al distrito.
- Dado que la fecha de cierre del período no está confirmada en el discovery, cuando el equipo inicia el sprint, entonces la rectora confirma la fecha exacta de entrega al distrito (Supuesto SA-03: la fecha límite es el 15 de julio de 2026; debe ser confirmada con la rectora al inicio del sprint).

Dependencias: US-01, US-02, US-03, US-08.
Origen: `us:US-07`, `mvp:resultado-esperado`, `mvp:metrica-exito`, `pain:documentos-urgentes-sin-entregar`

---

## Épica E-03 · Plantilla única y mantenible de reportes

### US-05 · Plantilla única RDLC parametrizable por nivel y curso   ·   épica E-03   ·   5 pts

**Como** desarrollador, **quiero** una plantilla de reporte única y configurable por nivel y curso que sirva para cuadros trimestrales, finales y reportes de promoción, **para** que cuando el Ministerio cambie los lineamientos solo tenga que actualizar un archivo y el cambio se propague a todos los cursos sin tocar plantillas individuales.

Criterios de aceptación (Gherkin):

- Dado que existen N cursos, cuando se genera cualquier reporte, entonces todos comparten la misma plantilla base RDLC con parámetros configurables por curso, sin archivos RDLC separados por curso (Supuesto SA-04: se mantiene RDLC como motor de reportes y se refactoriza la estructura de plantillas hacia una única parametrizable; no se migra a otro motor de generación de reportes).
- Dado que se modifica la configuración de una materia en un curso, cuando se regeneran los reportes de ese curso, entonces el cambio se refleja sin afectar las plantillas de otros cursos.
- Dado que el Ministerio actualiza un lineamiento de formato, cuando se edita la plantilla única, entonces todos los reportes generados a continuación reflejan el cambio sin intervención adicional.

Dependencias: US-06.
Origen: `us:US-05`, `req:R-03`, `req:R-11`, `pain:rdlc-por-curso`, `pain:plantillas-no-dinamicas`

---

## Épica E-04 · Formato de reporte conforme a requisitos del distrito

### US-08 · Escalas de calificación automáticas por nivel educativo   ·   épica E-04   ·   4 pts

**Como** rectora, **quiero** que la lógica de calificaciones aplique automáticamente la escala correcta según el nivel educativo del estudiante, **para** que los reportes no requieran ajustes manuales al cambiar de nivel y cumplan los lineamientos del Ministerio de Educación.

Criterios de aceptación (Gherkin):

- Dado que se genera un cuadro de nivel inicial, cuando se visualiza, entonces muestra escalas cualitativas sin calificaciones numéricas.
- Dado que se genera un cuadro de primero o segundo de básica, cuando se visualiza, entonces muestra la escala simbólica (+A / -A / B-) definida en el requisito R-05.
- Dado que se genera un cuadro de tercero de básica en adelante o bachillerato, cuando se visualiza, entonces muestra calificaciones numéricas en la escala cuantitativa (Supuesto SA-05: escala de 0 a 10, aprobación con puntaje >= 7, conforme al reglamento del Ministerio de Educación; el desarrollador confirma los rangos exactos con la secretaria al inicio del sprint).
- Dado que un estudiante cambia de nivel en el sistema, cuando se regenera su reporte, entonces aplica automáticamente la escala del nuevo nivel sin configuración manual.

Dependencias: US-06, US-05.
Origen: `us:US-08`, `req:R-05`, `mvp:funcionalidades-minimas·4`, `pain:formato-reporte-inadecuado`

---

### US-03 · Espacios de firma y sellos en reportes y cuadros   ·   épica E-04   ·   3 pts

**Como** secretaria, **quiero** que los cuadros y reportes de promoción incluyan espacios designados para la firma del docente y dos sellos (colegio y distrito), **para** poder enviarlos al distrito sin modificaciones manuales en el documento impreso.

Criterios de aceptación (Gherkin):

- Dado que se genera cualquier reporte del período 2025-2026, cuando se visualiza o imprime, entonces aparecen los espacios de firma del docente y los dos sellos (colegio y distrito) en la posición definida por el formato institucional.
- Dado que el reporte contiene calificaciones de segundo de básica a bachillerato, cuando se envía al distrito, entonces el formato cumple los requisitos de presentación del distrito sin necesidad de edición adicional.
- Dado que se imprimen reportes de distintos cursos, cuando se comparan los espacios de firma y sello, entonces su posición y tamaño son consistentes entre documentos.
- Dado que no existen especificaciones formales del distrito para medidas y posición exacta de firma y sellos, cuando el desarrollador implementa el layout, entonces toma como referencia el formato que la secretaria usa actualmente y la secretaria valida el prototipo antes del sprint review (Supuesto SA-06: la posición y tamaño de firma y sellos se derivan del formato que edita manualmente la secretaria; no existe documento formal del distrito que los especifique).

Dependencias: US-05.
Origen: `us:US-03`, `req:R-06`, `mvp:funcionalidades-minimas·5`, `pain:formato-reporte-inadecuado`

---

### US-04 · Jerarquía visual secundaria para escalas de comportamiento   ·   épica E-04   ·   2 pts

**Como** secretaria, **quiero** que las secciones de escalas de calificación y comportamiento tengan jerarquía visual menor que los datos de calificaciones en el reporte impreso, **para** que la información principal de calificaciones sea fácil de leer y el reporte no esté dominado por las tablas de escala.

Criterios de aceptación (Gherkin):

- Dado que se genera e imprime un cuadro de calificaciones, cuando se visualiza, entonces la sección de calificaciones de los estudiantes ocupa más espacio visual que la sección de escalas de comportamiento.
- Dado que el reporte es de nivel inicial o básica inferior, cuando se visualiza, entonces la escala cualitativa o simbólica aparece en tamaño tipográfico secundario (Supuesto SA-07: fuente de 10pt o menor para escalas vs. 12pt para calificaciones) y la sección de escalas ocupa como máximo el 25 % del área imprimible del reporte; la secretaria valida estas proporciones en demo antes del sprint review.
- Dado que se comparan reportes de distintos cursos, cuando se imprimen, entonces la proporción visual entre datos de calificaciones y escalas es consistente en todos.

Dependencias: US-05.
Origen: `us:US-04`, `req:R-10`, `pain:formato-reporte-inadecuado`

---

## Resumen del backlog refinado

| ID     | Título corto                                         | Épica | Pts | Dependencias           |
|--------|------------------------------------------------------|-------|-----|------------------------|
| US-06  | Esquema de BD centralizado de calificaciones         | E-01  | 5   | —                      |
| US-06a | Migración de calificaciones al esquema centralizado  | E-01  | 5   | US-06                  |
| US-01  | Nombres de materias dinámicos en cuadros             | E-02  | 3   | US-06                  |
| US-02  | Aislamiento de cambios entre reportes del período    | E-02  | 3   | US-01, US-05           |
| US-07  | Disponibilidad de reportes 2025-2026 antes del cierre| E-02  | 2   | US-01, US-02, US-03, US-08 |
| US-05  | Plantilla única RDLC parametrizable                  | E-03  | 5   | US-06                  |
| US-08  | Escalas de calificación automáticas por nivel        | E-04  | 4   | US-06, US-05           |
| US-03  | Espacios de firma y sellos en reportes               | E-04  | 3   | US-05                  |
| US-04  | Jerarquía visual secundaria para escalas             | E-04  | 2   | US-05                  |
| **Total** |                                                   |       | **32** |                     |

---

## Supuestos adoptados

Los siguientes supuestos se formularon para resolver las preguntas abiertas que el PO dejó pendientes. Cada uno debe ser confirmado o refutado por el equipo al inicio del sprint; si se refuta, el criterio de aceptación correspondiente debe ajustarse antes de iniciar la implementación.

| ID    | Historia | Supuesto                                                                                                                                                                                                                   | Confirmación requerida de        |
|-------|----------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------|
| SA-01 | US-06    | No existe ambiente de staging formal en la institución. La validación del nuevo esquema se realiza sobre una copia local de la base de datos de producción antes de aplicar los cambios en producción.                      | Desarrollador (al inicio del sprint) |
| SA-02 | US-06a   | La migración puede incluir registros con datos vacíos o inconsistentes. El desarrollador ejecuta un script de validación previa, identifica y limpia esos registros. La secretaria revisa una muestra de calificaciones del período 2025-2026 para dar el visto bueno final antes de considerar la historia terminada. | Desarrollador + Secretaria       |
| SA-03 | US-07    | La fecha límite de entrega de documentos al distrito para el cierre del período 2025-2026 es el 15 de julio de 2026. El valor proviene de inferencia del discovery (el mvp-canvas lo describe como "inminente" sin fecha exacta); debe ser confirmado con la rectora al inicio del sprint. | Rectora (primer día del sprint)  |
| SA-04 | US-05    | Se mantiene RDLC como motor de generación de reportes. El trabajo consiste en refactorizar la estructura de plantillas existentes hacia una única plantilla parametrizable, sin migrar a otro motor de reportes. Esta decisión debe registrarse en un ADR. | Desarrollador + Arquitecto       |
| SA-05 | US-08    | La escala cuantitativa para tercero de básica en adelante y bachillerato va de 0 a 10, con aprobación a partir de 7, conforme al reglamento del Ministerio de Educación. Los rangos exactos y los símbolos de comportamiento deben ser confirmados con la secretaria al inicio del sprint. | Secretaria (al inicio del sprint) |
| SA-06 | US-03    | No existe un documento formal del distrito que especifique medidas y posición exacta de los espacios de firma y sellos. El layout se diseña tomando como referencia el formato que la secretaria edita actualmente de forma manual. La secretaria valida un prototipo antes del sprint review. | Secretaria (demo intermedia)     |
| SA-07 | US-04    | La jerarquía visual secundaria se implementa usando fuente de 10pt o menor para la sección de escalas frente a 12pt para los datos de calificaciones, y la sección de escalas no supera el 25 % del área imprimible del reporte. La secretaria valida estas proporciones en una demo antes del sprint review. | Secretaria (demo intermedia)     |

# Historias refinadas — Ciencia y Fe · Ciclo 2025-2026

> Definition of Ready verificada por dor-invest-gate
> Puntos totales: 34 pts · Historias listas: 7

---

## US-07 · Esquema unificado de calificaciones en la BD

**Epica:** E-01 · Fundacion de datos: esquema centralizado de calificaciones
**Como** Desarrollador, **quiero** que las calificaciones esten en un esquema unificado de la BD con identificadores de materia, curso y periodo, **para** que todos los reportes lean de la misma fuente y sean coherentes entre si.
**Estimacion:** 8 pts · **Prioridad:** 1

### Criterios de aceptacion

```gherkin
Escenario: Consulta coherente desde multiples modulos de reporte
  Dado un registro de calificacion en el esquema centralizado
  Cuando lo consulto desde cualquier modulo de reporte
  Entonces el valor y el nombre de materia son identicos en todos los documentos del mismo periodo
```

```gherkin
Escenario: Nuevo tipo de reporte sin duplicacion de tablas
  Dado el nuevo esquema centralizado
  Cuando agrego un nuevo tipo de reporte al sistema
  Entonces no se requiere duplicar tablas de calificaciones para soportarlo
```

```gherkin
Escenario: Actualizacion de nombre de materia propagada a todos los reportes
  Dado el esquema centralizado con una materia registrada
  Cuando se actualiza el nombre de esa materia en la BD
  Entonces todos los reportes del periodo reflejan el nombre actualizado sin modificar procedimientos almacenados individuales
```

**Dependencias:** Ninguna
**Trazabilidad:** us:US-07, req:R-07, pain:inconsistencia-bd, pain:sp-no-mantenibles, pain:reportes-no-dinamicos

> Nota: el ciclo 2025-2026 parte con datos frescos en el esquema centralizado. La migracion de historicos de ciclos anteriores es un supuesto abierto documentado como riesgo en architecture.md; no es bloqueante de esta historia. (MVP Canvas — Riesgos/supuestos item 1.)

---

## US-01 · Cuadro de calificaciones trimestral con nombres desde la BD

**Epica:** E-02 · Reportes de calificaciones correctos en el primer intento
**Como** Secretaria, **quiero** generar el cuadro de calificaciones trimestral de cualquier curso con los nombres de materias obtenidos de la base de datos, **para** poder enviarlo al distrito sin errores de nomenclatura.
**Estimacion:** 5 pts · **Prioridad:** 2

### Criterios de aceptacion

```gherkin
Escenario: Nombres de materias provienen de la BD, no de texto quemado
  Dado un curso y un trimestre seleccionados
  Cuando genero el cuadro de calificaciones
  Entonces los nombres de materias coinciden exactamente con los registrados en la BD y no provienen de texto quemado en la plantilla
```

```gherkin
Escenario: Cambio de nombre de materia se refleja sin modificar plantillas
  Dado que se actualiza el nombre de una materia en la BD
  Cuando regenero el cuadro trimestral de cualquier curso
  Entonces el nuevo nombre aparece en el documento sin modificar ninguna plantilla ni procedimiento almacenado
```

```gherkin
Escenario: Cuadro incluye todos los estudiantes del curso con sus calificaciones
  Dado un curso valido registrado en la BD y un trimestre seleccionado
  Cuando genero el cuadro trimestral
  Entonces el documento incluye todos los estudiantes del curso con sus calificaciones del periodo seleccionado
```

**Dependencias:** US-07
**Trazabilidad:** us:US-01, req:R-01, pain:materias-quemadas, pain:documentos-urgentes-cierre-ano

> Nota: el cuadro quimestral es un sub-tipo del mismo reporte (mismo template RDLC, diferente periodo). US-01 cubre el trimestral. El quimestral se genera parametrizando el campo periodo con el valor 'quimestre'; no requiere historia separada. (req:R-01, MVP Canvas — Funcionalidades minimas item 2.)

---

## US-02 · Cuadro de calificaciones final coherente con los trimestrales

**Epica:** E-02 · Reportes de calificaciones correctos en el primer intento
**Como** Secretaria, **quiero** generar el cuadro de calificaciones final que sea coherente con los cuadros trimestrales del mismo periodo, **para** poder entregar documentos consistentes al distrito.
**Estimacion:** 5 pts · **Prioridad:** 3

### Criterios de aceptacion

```gherkin
Escenario: Promedios del cuadro final coinciden con los cuadros trimestrales
  Dado un curso y un periodo seleccionados
  Cuando genero el cuadro final de calificaciones
  Entonces los promedios coinciden con los valores registrados en los cuadros trimestrales de ese periodo
```

```gherkin
Escenario: Alerta de conflicto antes de producir el documento
  Dado una inconsistencia detectada en la BD entre registros del periodo
  Cuando intento generar el cuadro final
  Entonces el sistema alerta del conflicto antes de producir el documento y no genera un cuadro con datos inconsistentes
```

```gherkin
Escenario: Nombres de materias identicos entre cuadro final y trimestrales
  Dado el cuadro final generado para un curso y periodo
  Cuando lo comparo con los cuadros trimestrales del mismo periodo
  Entonces los nombres de materias son identicos en ambos documentos
```

**Dependencias:** US-07, US-01
**Trazabilidad:** us:US-02, req:R-02, pain:materias-quemadas, pain:documentos-urgentes-cierre-ano

---

## US-05 · Escala de calificacion diferenciada automatica por nivel

**Epica:** E-02 · Reportes de calificaciones correctos en el primer intento
**Como** Secretaria, **quiero** que el sistema aplique automaticamente la escala de calificacion correcta segun el nivel del curso, **para** que los reportes cumplan los lineamientos del Ministerio de Educacion sin intervencion manual.
**Estimacion:** 3 pts · **Prioridad:** 4

### Criterios de aceptacion

```gherkin
Escenario: Escala cualitativa para inicial y basica elemental (calificaciones academicas)
  Dado un estudiante de inicial o basica elemental (hasta 2 grado)
  Cuando genero su cuadro de calificaciones
  Entonces las calificaciones academicas aparecen en escala cualitativa (AS / A / EP / I) conforme al reglamento MINEDUC
```

```gherkin
Escenario: Escala numerica para basica media en adelante (calificaciones academicas)
  Dado un estudiante de basica media en adelante (desde 3 grado)
  Cuando genero su cuadro de calificaciones
  Entonces las calificaciones academicas aparecen en escala numerica del 1 al 10
```

```gherkin
Escenario: Seleccion automatica de escala segun nivel registrado en la BD
  Dado que el nivel del curso esta registrado en la BD
  Cuando el sistema genera el cuadro de calificaciones
  Entonces selecciona la escala correcta automaticamente sin requerir configuracion manual por reporte
```

```gherkin
Escenario: Escala de comportamiento segun nivel del curso
  Dado un estudiante de inicial o basica elemental (hasta 2 grado)
  Cuando genero su cuadro
  Entonces la columna de comportamiento muestra escala cualitativa (AS / A / EP / I)
  Y dado un estudiante de basica media en adelante (desde 3 grado)
  Cuando genero su cuadro
  Entonces la columna de comportamiento muestra escala numerica del 1 al 10
```

**Dependencias:** US-07
**Trazabilidad:** us:US-05, req:R-05, pain:materias-quemadas, pain:documentos-urgentes-cierre-ano

> Supuesto aplicado: escala AS/A/EP/I para comportamiento en inicial y basica elemental; numerica 10-1 en basica media en adelante, conforme al reglamento MINEDUC (2016). Si el colegio aplica valores distintos al reglamento estandar, ese delta es un supuesto abierto para el Architect en architecture.md. (req:R-05)

---

## US-03 · Reporte de promociones con nombres de materias desde la BD

**Epica:** E-03 · Reporte de promociones consistente con los cuadros de calificaciones
**Como** Secretaria, **quiero** generar el reporte de promociones con los nombres de materias provenientes de la base de datos y coincidentes con los del cuadro de calificaciones, **para** evitar inconsistencias entre documentos entregados al distrito.
**Estimacion:** 5 pts · **Prioridad:** 5

### Criterios de aceptacion

```gherkin
Escenario: Materias del reporte de promociones coinciden con el cuadro de calificaciones
  Dado un curso seleccionado
  Cuando genero el reporte de promociones
  Entonces cada materia listada comparte el mismo identificador y nombre que en el cuadro de calificaciones de ese curso
```

```gherkin
Escenario: Llenado por referencia a la BD, sin posicionamiento
  Dado el reporte de promociones generado para un curso
  Cuando lo reviso celda por celda
  Entonces no existe llenado por posicionamiento: cada celda se llena por referencia a su materia en la BD
```

```gherkin
Escenario: Cambio de nombre de materia reflejado en reporte de promociones
  Dado un cambio de nombre de materia en la BD
  Cuando regenero el reporte de promociones
  Entonces el nombre actualizado aparece en el documento sin modificar la plantilla ni el procedimiento almacenado
```

**Dependencias:** US-07
**Trazabilidad:** us:US-03, req:R-03, pain:plantillas-word-quemadas-promociones

---

## US-04 · Espacios de firma y sello en todos los reportes

**Epica:** E-04 · Documentos formalmente validos ante el distrito
**Como** Secretaria, **quiero** que los reportes incluyan espacio reservado para la firma del docente y los sellos del colegio y del distrito, **para** que los documentos cumplan los requisitos formales de entrega al distrito.
**Estimacion:** 2 pts · **Prioridad:** 6

### Criterios de aceptacion

```gherkin
Escenario: Reporte incluye espacios de firma y sello en posiciones estandar
  Dado cualquier tipo de reporte impreso (calificaciones o promociones)
  Cuando lo genero
  Entonces el documento incluye el espacio de firma del docente, el sello del colegio y el sello del distrito en posiciones estandar
```

```gherkin
Escenario: Placeholder visible cuando no existe imagen digital del sello
  Dado que el colegio no ha provisto una imagen digital del sello
  Cuando genero el reporte
  Entonces el sistema muestra un area reservada con borde visible (placeholder) en la posicion del sello, de modo que el documento cumple el requisito formal de espacio sin bloquear la generacion
```

```gherkin
Escenario: Imagen digital del sello sustituye el placeholder cuando se configura
  Dado que el colegio provee una imagen digital del sello (PNG o JPG)
  Cuando se configura el parametro de imagen en la plantilla RDLC
  Entonces el sello aparece en el reporte en sustitucion del placeholder
```

**Dependencias:** US-07
**Trazabilidad:** us:US-04, req:R-04, pain:documentos-urgentes-cierre-ano

> Decision de MVP: el espacio de firma y sello se implementa como area reservada con borde (placeholder visual) en la plantilla RDLC. El activo digital no es bloqueante del MVP; el placeholder cumple R-04. (req:R-04, us:US-04)

---

## US-06 · Plantilla de reporte parametrizada y compartida por todos los cursos

**Epica:** E-05 · Plataforma de reportes mantenible: plantilla unica parametrizada
**Como** Desarrollador, **quiero** usar una plantilla de reporte parametrizada compartida por todos los cursos, **para** que un cambio de diseno o estructura aplique a todos sin tocar archivos individuales.
**Estimacion:** 6 pts · **Prioridad:** 7

### Criterios de aceptacion

```gherkin
Escenario: Cambio de diseno en plantilla unica se propaga a todos los cursos
  Dado un cambio en un elemento de diseno (logo, encabezado) aplicado a la plantilla unica
  Cuando actualizo la plantilla
  Entonces todos los reportes de todos los cursos reflejan el cambio en la siguiente generacion sin tocar archivos adicionales
```

```gherkin
Escenario: Nuevo curso usa la plantilla compartida sin crear template adicional
  Dado el registro de un nuevo curso en la BD
  Cuando solicito su reporte
  Entonces el sistema lo genera con la plantilla compartida sin necesidad de crear un template adicional para ese curso
```

```gherkin
Escenario: Reduccion de archivos de plantilla al implementar la plantilla parametrizada
  Dado el sistema actual con plantillas RDLC individuales por curso
  Cuando se implementa la plantilla parametrizada
  Entonces el numero de archivos de plantilla se reduce a uno por tipo de reporte independientemente del numero de cursos registrados
```

**Dependencias:** US-07
**Trazabilidad:** us:US-06, req:R-06, req:R-12, pain:rdlc-por-curso, pain:reportes-no-dinamicos

---

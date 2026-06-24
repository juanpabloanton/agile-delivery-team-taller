# ADR-0002 — Esquema de BD centralizado (una fuente de verdad para calificaciones)

- **Estado:** Aceptado
- **Fecha:** 2026-06-24
- **Autores:** Architect · Agile Delivery Team

## Contexto

El dolor `pain:inconsistencia-bd` documenta que las calificaciones hoy están
distribuidas en múltiples tablas desconectadas entre sí. El pain
`pain:sp-no-mantenibles` documenta que los stored procedures contienen lógica de
posicionamiento porque no pueden confiar en identificadores de materia comunes.
El pain `pain:materias-quemadas` documenta que los nombres de materias están en
texto fijo dentro de las plantillas y los SPs, en lugar de provenir de la BD.

El requisito `req:R-07` exige "centralizar las calificaciones en un esquema
unificado y coherente que sirva a todos los cursos y tipos de reporte". La historia
`us:US-07` es el prerequisito técnico explícito de todas las demás historias del
MVP: sin un esquema centralizado, los reportes siguen leyendo fuentes
inconsistentes y el outcome del 90 % de aceptación es inalcanzable.

## Decisión

Se crea un esquema relacional centralizado en SQL Server con las siguientes
entidades mínimas: `Nivel`, `Curso`, `Periodo`, `Materia`, `Estudiante` y
`Calificacion`. Todas las consultas de todos los reportes del ciclo 2025-2026
leen exclusivamente de este esquema. Los stored procedures se reescriben como
consultas parametrizadas sin lógica de posicionamiento ni nombres de materias
en texto fijo.

Esta es la intervención estructural mínima que elimina los tres dolores principales
(`pain:inconsistencia-bd`, `pain:materias-quemadas`, `pain:sp-no-mantenibles`) en
un único lugar, sin requerir tecnología adicional.

## Alternativas consideradas

| Alternativa | Por qué se descartó |
|-------------|---------------------|
| Vistas SQL sobre tablas existentes | Es un parche que oculta la inconsistencia sin eliminarla. Los nombres de materias seguirían quemados en algún lugar. No resuelve `pain:inconsistencia-bd` ni permite la trazabilidad de `req:R-07`. |
| Migración total con ORM (Entity Framework) | Sobredimensionado para un equipo de dos desarrolladores con fecha límite fija. Introduce una capa adicional de complejidad sin aportar valor al problema concreto de los reportes. |
| Mantener el esquema actual y solo actualizar las plantillas | No resuelve la raíz del problema: los datos seguirían siendo inconsistentes entre tablas; solo cambia dónde se ve el error. |

## Consecuencias

**Positivas:**
- Un cambio de nombre de materia en la tabla `Materia` se propaga
  automáticamente a todos los reportes (trimestrales, finales, promociones) sin
  modificar ninguna plantilla ni ningún SP.
- Agregar un nuevo tipo de reporte no requiere duplicar tablas de calificaciones
  (criterio de aceptación de US-07).
- Los SPs se vuelven mantenibles: reciben parámetros (`@CursoId`, `@PeriodoId`)
  y retornan un `DataSet` tipado; sin posicionamiento, sin texto quemado.

**Negativas / riesgos:**
- La migración de datos históricos de calificaciones de ciclos anteriores al
  esquema centralizado es un supuesto no validado (MVP Canvas — Riesgos/supuestos,
  ítem 1). El MVP 2025-2026 parte con datos frescos; los históricos quedan en el
  esquema legado hasta que se valide un script de migración. Este riesgo está
  documentado explícitamente en `architecture.md` como supuesto abierto.
- El esquema centralizado requiere que el equipo cargue los datos del ciclo
  2025-2026 desde el inicio (estudiantes, materias, cursos, períodos) antes de
  que los reportes sean operativos. Esta tarea de carga inicial no está estimada
  en el backlog y debe planificarse como actividad de despliegue.

## Trazabilidad

- Origina: `pain:inconsistencia-bd` · `pain:materias-quemadas` ·
  `pain:sp-no-mantenibles` · `req:R-07` · `us:US-07`
- Impacta: US-01, US-02, US-03, US-04, US-05, US-06 (todas dependen de US-07)

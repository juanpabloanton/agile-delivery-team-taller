# ADR-0002 · Tabla unificada de calificaciones en lugar de tablas independientes por curso

**Estado:** aceptado  
**Fecha:** 2026-06-23

## Contexto y fuerza

El esquema de base de datos actual está fragmentado: existen tablas de calificaciones independientes por curso o por variante de reporte. El resultado son stored procedures que tienen hasta 30 000 líneas con `if` anidados, donde "un SP llama a otro y arreglar uno rompe otro" (`personas.md` · Desarrollador · `pain:sp-no-mantenibles`). La rectora describe el mismo problema desde su perspectiva: "la información no está centralizada: no existe un único esquema de BD ni una tabla unificada de calificaciones" (`personas.md` · Rectora · `pain:informacion-no-centralizada`).

El requisito R-04 exige explícitamente "un esquema de base de datos centralizado para calificaciones, con una sola tabla unificada que sirva a todos los cursos y reportes" (`requisitos.md` · R-04). El MVP canvas lista esta centralización como funcionalidad mínima número 1 (`mvp-canvas.md` · funcionalidades-minimas·1). La historia US-06 y su historia de migración US-06a tienen a este esquema como prerequisito de todo el motor de reportes (`epics.md` · E-01, `stories.md` · US-06).

El requisito R-11 (no funcional) establece que "cualquier cambio en plantillas, nombres de materias o escalas debe realizarse en un único punto" (`requisitos.md` · R-11), lo que es imposible de cumplir con datos fragmentados.

## Decisión

Se diseña una tabla centralizada de calificaciones que consolida los registros de todos los cursos y períodos en una única estructura, con columnas discriminadoras (nivel educativo, curso, período, materia, tipo de reporte). Todos los stored procedures de consulta para generación de reportes se escriben contra esta tabla o contra una vista construida sobre ella, eliminando la proliferación de SPs por variante de curso.

## Alternativas consideradas

- **Mantener tablas por curso y crear una capa de abstracción (vistas unificadas sobre las tablas actuales)** — Reduce el esfuerzo de migración inicial, pero no elimina la duplicidad de estructuras; el mantenimiento del esquema sigue siendo fragmentado y se acumula deuda técnica. Esta opción contradice directamente R-04 y el supuesto de mayor riesgo del MVP canvas (`mvp-canvas.md` · riesgos·1).
- **Un esquema por nivel educativo (inicial / básica / bachillerato)** — Reduce la fragmentación respecto al estado actual pero sigue requiriendo múltiples puntos de modificación cuando el Ministerio cambia lineamientos, lo que contradice R-11. El discovery no aporta evidencia que justifique esta granularidad intermedia.

## Consecuencias

**Ganamos:** una única lógica de consulta reutilizable para todos los reportes (US-06, criterio 2); eliminación de SPs duplicados (`pain:sp-no-mantenibles`); coherencia garantizada entre cuadro trimestral, cuadro final y reporte de promoción para el mismo estudiante (`us:US-02`, `req:R-02`); base sólida para que E-02, E-03 y E-04 puedan construirse sin duplicar lógica de datos.

**Aceptamos:** la migración de datos del esquema fragmentado al centralizado es el riesgo más alto del MVP (`mvp-canvas.md` · riesgos·1); este riesgo se gestiona con la historia US-06a y su script de validación previa. No existe ambiente de staging formal (supuesto SA-01, `stories.md`), por lo que la validación se realiza sobre copia local antes de aplicar en producción, lo que introduce un riesgo operativo menor pero controlado.

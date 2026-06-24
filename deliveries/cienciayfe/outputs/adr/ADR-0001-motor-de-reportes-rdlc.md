# ADR-0001 · Mantener RDLC como motor de generación de reportes y refactorizar hacia una plantilla única parametrizable

**Estado:** aceptado  
**Fecha:** 2026-06-23

## Contexto y fuerza

El sistema académico actual genera cuadros trimestrales, finales y reportes de promoción usando archivos RDLC individuales por curso. El desarrollador describe el problema directamente: "cada curso tiene su propio RDLC; el editor RDLC no es amigable y cualquier movimiento desplaza todo el layout" (`personas.md` · Desarrollador · `pain:rdlc-por-curso`). Los nombres de materias están "quemados" en los RDLC (`personas.md` · Desarrollador · `pain:plantillas-no-dinamicas`), lo que obliga a editar archivo por archivo cuando el Ministerio cambia los lineamientos (`personas.md` · Rectora).

El supuesto SA-04 del refinamiento de US-05 (`stories.md`) dice explícitamente: "se mantiene RDLC como motor de reportes y se refactoriza la estructura de plantillas hacia una única parametrizable; no se migra a otro motor". El canvas del MVP lista como funcionalidad mínima: "Motor de reportes con plantilla única parametrizable por nivel y curso (sin RDLC duplicados)" (`mvp-canvas.md` · funcionalidades-minimas·2). El requisito R-03 refuerza la misma dirección: "plantilla única de reporte reutilizable y configurable por curso, sin necesidad de duplicar archivos RDLC o Word por nivel" (`requisitos.md` · R-03).

La institución tiene dos desarrolladores con carga urgente de entrega para el cierre 2025-2026; migrar a un motor distinto implicaría una curva de aprendizaje y un riesgo de no entregar a tiempo (`mvp-canvas.md` · riesgos·2, `personas.md` · Rectora · `pain:documentos-urgentes-sin-entregar`).

## Decisión

Se mantiene RDLC como motor de generación de reportes. El trabajo de construcción consiste en refactorizar la estructura de archivos RDLC actuales hacia una única plantilla base parametrizable que reciba como parámetros el nivel educativo, el curso y los nombres de materias leídos desde la base de datos, sin duplicar el archivo por cada curso.

## Alternativas consideradas

- **Migrar a otro motor de reportes (FastReport, Crystal Reports, SSRS nativo, Telerik Reporting)** — Requiere reescribir todas las plantillas existentes, aprender una nueva herramienta y garantizar compatibilidad con el stack .NET actual; el riesgo de no entregar para el cierre 2025-2026 es demasiado alto dado que los documentos urgentes aún no están listos (`rectora.md` · `pain:documentos-urgentes-sin-entregar`). El discovery no aporta evidencia de que el equipo haya evaluado estas herramientas.
- **Generar reportes desde plantillas Word/HTML** — El llenado de plantillas Word actualmente se hace "por posicionamiento, sin lógica de negocio" (`personas.md` · Desarrollador), lo que reproduce el problema. El canvas no menciona esta alternativa y el requisito R-03 apunta a eliminar el problema Word, no a formalizarlo.

## Consecuencias

**Ganamos:** continuidad sobre la inversión técnica existente en RDLC; reducción inmediata del número de archivos a mantener (de N archivos por curso a una plantilla única); capacidad de cumplir el plazo del período 2025-2026 sin una curva de aprendizaje nueva; trazabilidad directa a R-03, R-11 y US-05.

**Aceptamos:** el editor RDLC sigue siendo poco ergonómico para cambios de layout (dolor reconocido en el discovery); si en el futuro el equipo crece o el Ministerio exige formatos más complejos, la deuda de cambiar de motor seguirá presente. Esta consecuencia se registra como pregunta abierta para el siguiente ciclo de planificación.

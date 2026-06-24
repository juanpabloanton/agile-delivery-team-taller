# ADR-0003 · Migración incremental con validación en copia local antes de aplicar en producción

**Estado:** aceptado  
**Fecha:** 2026-06-23

## Contexto y fuerza

El supuesto SA-02 de US-06a (`stories.md`) identifica la migración de datos como "el riesgo mayor del MVP": las calificaciones del período 2025-2026 están distribuidas en el esquema fragmentado actual y deben trasladarse íntegramente al esquema centralizado sin pérdida ni corrupción, bajo presión de tiempo (`mvp-canvas.md` · riesgos·1). El MVP canvas describe este supuesto de migración como "el de mayor riesgo" y advierte que debe ser "limpia y completa" (`mvp-canvas.md` · riesgos·1).

Hay dos agravantes concretos documentados en el discovery:

1. La calidad del esquema fragmentado es incierta: los stored procedures tienen hasta 30 000 líneas con lógica enredada (`personas.md` · Desarrollador · `pain:sp-no-mantenibles`), lo que implica que los datos pueden tener inconsistencias acumuladas.
2. No existe ambiente de staging formal (supuesto SA-01, `stories.md`); el único entorno disponible para pruebas es una copia local de la base de datos de producción.

La historia US-06a exige que: el 100 % de registros del período 2025-2026 estén presentes en el esquema centralizado al finalizar; los recuentos por curso coincidan entre esquema antiguo y nuevo; no se registren errores de integridad referencial; y la secretaria valide una muestra de calificaciones antes de dar el visto bueno final (`stories.md` · US-06a).

## Decisión

La migración se ejecuta en dos fases secuenciales y separadas:

1. **Fase de validación previa (antes de tocar producción):** el desarrollador ejecuta un script de auditoría sobre una copia local de la base de datos de producción. El script identifica y reporta registros con datos vacíos, valores fuera de rango o inconsistencias de integridad referencial. El desarrollador limpia y normaliza esos registros antes de proceder. El criterio de salida de esta fase es un log de auditoría sin errores críticos.

2. **Fase de migración y verificación:** se ejecuta el script de migración sobre la copia local validada y, una vez que los recuentos por curso coinciden entre esquema antiguo y nuevo, se aplica en producción fuera del horario de uso del sistema. La secretaria revisa una muestra representativa de calificaciones del período 2025-2026 en el esquema centralizado antes de que el equipo declare US-06a como terminada.

Las tablas del esquema fragmentado no se eliminan hasta que los reportes del período 2025-2026 hayan sido aprobados y enviados al distrito, como red de seguridad de reversión.

## Alternativas consideradas

- **Migración en un solo paso directo a producción sin validación previa** — Inaceptable dado el riesgo documentado de inconsistencias en el esquema actual (`pain:sp-no-mantenibles`) y la ausencia de staging (`SA-01`); un fallo en producción durante el cierre de período equivale a pérdida de datos críticos para el distrito.
- **Migración paralela: mantener los dos esquemas activos simultáneamente y sincronizarlos** — La dualidad de escritura requiere lógica de sincronización adicional que aumenta la complejidad sin reducir el riesgo en el plazo del MVP; el mvp-canvas no contempla esta complejidad y el equipo de dos personas no tiene capacidad para implementarla junto con el módulo de reportes (`mvp-canvas.md` · riesgos·2).
- **Delegar la limpieza de datos a la secretaria antes de la migración** — La secretaria no tiene acceso ni conocimiento del esquema de base de datos; el discovery la describe como responsable de validar la información presentada en los reportes, no los datos en bruto. La validación técnica es responsabilidad del desarrollador (SA-02, `stories.md`).

## Consecuencias

**Ganamos:** control explícito del riesgo más alto del MVP antes de afectar producción; trazabilidad completa (log de auditoría previo y log de migración); la secretaria tiene voz formal en la aceptación de la migración (SA-02), lo que reduce el riesgo de devoluciones post-entrega por calificaciones incorrectas; las tablas antiguas quedan disponibles como respaldo hasta el cierre del período.

**Aceptamos:** la fase de validación previa añade tiempo de esfuerzo (estimado como parte de los 5 puntos de US-06a); la ejecución fuera de horario implica coordinación con la institución para garantizar ventana de mantenimiento; el script de auditoría debe ser escrito y mantenido por el desarrollador, lo que es un esfuerzo adicional no menor. La ausencia de staging formal (SA-01) queda como deuda de infraestructura para ciclos futuros.

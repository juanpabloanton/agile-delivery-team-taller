# Sprint 1 — Centralizar los datos y la plantilla base para poder generar el primer reporte del período 2025-2026 sin dependencias del esquema fragmentado

**Capacidad:** 20 pts · **Comprometido:** 20 pts

---

## Historias comprometidas

| Historia | Título                                                   | Épica | Pts | Prioridad |
|----------|----------------------------------------------------------|-------|-----|-----------|
| US-06    | Esquema de BD centralizado de calificaciones             | E-01  | 5   | 1         |
| US-06a   | Migración de calificaciones al esquema centralizado      | E-01  | 5   | 2         |
| US-01    | Nombres de materias dinámicos en cuadros                 | E-02  | 3   | 3         |
| US-05    | Plantilla única RDLC parametrizable por nivel y curso    | E-03  | 5   | 6         |
| US-04    | Jerarquía visual secundaria para escalas de comportamiento | E-04 | 2   | 9         |

**Total comprometido: 20 pts**

Orden de implementación recomendado: US-06 → US-06a (en paralelo con US-01 y US-05 una vez US-06 esté done) → US-01 → US-05 → US-04.

---

## Historias diferidas (siguiente sprint)

| Historia | Título                                                    | Épica | Pts | Motivo de diferimiento                                          |
|----------|-----------------------------------------------------------|-------|-----|-----------------------------------------------------------------|
| US-08    | Escalas de calificación automáticas por nivel educativo   | E-04  | 4   | Sin capacidad; depende de US-06 y US-05 (resueltas en Sprint 1) |
| US-02    | Aislamiento de cambios entre reportes del mismo período   | E-02  | 3   | Sin capacidad; depende de US-01 y US-05 (resueltas en Sprint 1) |
| US-03    | Espacios de firma y sellos en reportes y cuadros          | E-04  | 3   | Sin capacidad; depende de US-05 (resuelta en Sprint 1)          |
| US-07    | Disponibilidad de todos los reportes 2025-2026 antes del cierre | E-02 | 2 | Dependencias US-02, US-03 y US-08 no resueltas en Sprint 1     |

**Total diferido: 12 pts** (candidatos a Sprint 2, con todas las dependencias ya satisfechas al cierre de Sprint 1).

---

## Supuestos a confirmar al inicio del sprint

Los siguientes supuestos están adoptados en las historias comprometidas. Si alguno se refuta, el criterio de aceptación correspondiente debe ajustarse antes de iniciar la implementación de la historia afectada.

- **SA-01 (US-06):** No existe ambiente de staging formal. La validación del nuevo esquema se realiza sobre una copia local de la base de datos de producción. Confirmar con el desarrollador el primer día del sprint.
- **SA-02 (US-06a):** La migración puede incluir registros con datos vacíos o inconsistentes. El desarrollador ejecuta un script de validación previa y la secretaria revisa una muestra de calificaciones del período 2025-2026 antes de dar el visto bueno final. Confirmar con desarrollador y secretaria antes de ejecutar la migración.
- **SA-03 (US-07 — diferida pero con impacto en el plazo del sprint):** La fecha límite de entrega al distrito es el 15 de julio de 2026. Debe ser confirmada con la rectora el primer día del sprint para determinar si la capacidad y el alcance del Sprint 2 necesitan ajustarse.
- **SA-04 (US-05):** Se mantiene RDLC como motor de reportes. El trabajo consiste en refactorizar las plantillas existentes hacia una única plantilla parametrizable. Confirmar con desarrollador y arquitecto al inicio del sprint (ya registrado en ADR-0001).
- **SA-07 (US-04):** La jerarquía visual secundaria usa fuente de 10 pt o menor para escalas frente a 12 pt para calificaciones, y la sección de escalas no supera el 25 % del área imprimible. La secretaria valida estas proporciones en una demo intermedia antes del sprint review.

---

## Riesgos del sprint

1. **Riesgo de migración (ADR-0003 / US-06a — ALTO).**
   El esquema actual contiene stored procedures de hasta 30 000 líneas y tablas de calificaciones fragmentadas por curso. La migración al esquema centralizado es el riesgo técnico más alto del MVP. Mitigación: ejecutar la migración sobre una copia local de producción (SA-01), correr el script de validación previa antes de tocar producción (SA-02), obtener el visto bueno de la secretaria sobre una muestra antes de declarar US-06a como Done. No ejecutar la migración en producción sin el informe de validación completo.

2. **Riesgo de plazo de cierre (SA-03 / US-07 — ALTO).**
   La fecha de entrega de documentos al distrito para el cierre 2025-2026 no está confirmada en el discovery. Si la fecha real es anterior al 15 de julio de 2026, el Sprint 2 (que contiene US-08, US-02, US-03 y US-07) queda sin margen. Mitigación: confirmar la fecha con la rectora el primer día del sprint y ajustar la priorización del Sprint 2 de inmediato si es necesario.

3. **Riesgo de alcance de US-05 (plantilla RDLC — MEDIO).**
   La refactorización de múltiples plantillas RDLC por curso hacia una única parametrizable puede revelar variaciones de formato no documentadas entre niveles. Mitigación: el desarrollador hace un inventario de plantillas existentes en los primeros dos días del sprint antes de comenzar la refactorización; si aparecen variaciones bloqueantes, se lleva al daily para renegociar el alcance.

4. **Riesgo de disponibilidad de la secretaria para validaciones (MEDIO).**
   US-06a y US-04 requieren demos intermedias con la secretaria (SA-02, SA-07). Si la secretaria no tiene disponibilidad antes del sprint review, las historias no pueden declararse Done. Mitigación: agendar las sesiones de validación al inicio del sprint, no al final.

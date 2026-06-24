# ADR-0003 — Estrategia de migración: datos frescos para el MVP 2025-2026

- **Estado:** Aceptado
- **Fecha:** 2026-06-24
- **Autores:** Architect · Agile Delivery Team

## Contexto

El MVP Canvas del Colegio Ciencia y Fe documenta explícitamente en su bloque de
riesgos y supuestos: "La BD actual puede migrarse a un esquema centralizado sin
pérdida de datos históricos de calificaciones". Esta afirmación es un supuesto
no validado: no existe en el inbox ninguna evidencia de entrevista, script de
migración ni análisis del esquema legado que confirme que la migración es segura.

La historia `us:US-07` (prerequisito técnico de todas las demás) incluye en su
campo `notes` la decisión de que "el ciclo 2025-2026 parte con datos frescos
registrados directamente en el esquema centralizado. La migración de históricos
es un supuesto abierto documentado como riesgo en architecture.md".

El equipo tiene dos desarrolladores y el ciclo 2025-2026 tiene una fecha de
cierre fija. Una migración de datos históricos mal ejecutada puede corromper los
registros de ciclos anteriores y bloquear la operación del colegio.

## Decisión

El MVP 2025-2026 arranca con el esquema centralizado vacío. Todos los datos del
ciclo 2025-2026 (niveles, cursos, períodos, materias, estudiantes, calificaciones)
se ingresan directamente en el nuevo esquema desde el inicio del ciclo. Los datos
históricos de ciclos anteriores permanecen accesibles en el sistema legado tal
como están hoy, sin ser migrados, hasta que se valide un proceso de migración
formal en una segunda fase fuera del alcance del MVP.

Esta decisión elimina el riesgo de pérdida de datos históricos y permite al equipo
entregar el sistema funcional antes del cierre del ciclo sin bloqueos por
dependencias de datos del pasado.

## Alternativas consideradas

| Alternativa | Por qué se descartó |
|-------------|---------------------|
| Migración completa de históricos antes del MVP | Riesgo no aceptable: no hay evidencia de que el esquema legado sea estructuralmente compatible con el nuevo esquema centralizado. Un error en la migración bloquea el ciclo escolar. Requiere tiempo de análisis y validación que no está disponible antes de la fecha de cierre. |
| Convivencia de esquema viejo y nuevo durante el MVP | Duplica la inconsistencia que se quiere eliminar. Requiere mantener dos fuentes de datos activas, lo que significa que los SPs deben decidir de cuál leer: esto reproduce exactamente `pain:inconsistencia-bd` en otra forma. |
| Migración parcial (solo ciclo anterior) | Introduce un estado híbrido sin beneficio claro: el colegio no necesita reportes del ciclo anterior desde el nuevo sistema para cumplir el objetivo del MVP 2025-2026. |

## Consecuencias

**Positivas:**
- El equipo puede arrancar el desarrollo del ciclo 2025-2026 sin bloqueo por
  análisis de datos históricos.
- No hay riesgo de pérdida de calificaciones históricas: el sistema legado las
  conserva intactas.
- Los reportes del ciclo 2025-2026 son 100 % coherentes desde el primer día
  porque todos los datos provienen del esquema centralizado.

**Negativas / riesgos:**
- Los reportes de ciclos anteriores al 2025-2026 solo son accesibles desde el
  sistema legado mientras no se ejecute la segunda fase de migración. El colegio
  debe mantener el sistema legado operativo en paralelo por el tiempo que
  necesite acceder a esos históricos.
- La carga inicial de datos del ciclo 2025-2026 (materias, cursos, estudiantes)
  en el esquema centralizado es una tarea manual o semi-automatizada que debe
  planificarse antes del inicio del ciclo. No está estimada en el backlog actual.
- Si el Ministerio de Educación solicita reportes históricos consolidados que
  crucen datos de ciclos anteriores con el 2025-2026, esa consulta no será
  posible desde el nuevo sistema hasta que se ejecute la migración.

## Trazabilidad

- Origina: MVP Canvas — Riesgos/supuestos ítem 1 · `us:US-07` (campo
  `open_questions` y `notes`) · `pain:inconsistencia-bd`
- Impacta: US-07 (desfuerza el bloqueo de migración como prerequisito), E-01

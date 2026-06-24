# ADR-0004 · Diferir el despliegue con Docker Compose al siguiente ciclo de planificación

**Estado:** propuesto  
**Fecha:** 2026-06-23

## Contexto y fuerza

El requisito R-13 establece que "los módulos del sistema nuevo deben desplegarse como contenedores Docker con un archivo `docker-compose` que permita levantar el ecosistema completo en un solo comando" (`requisitos.md` · R-13). Este requisito fue mencionado por el desarrollador y la rectora como deseable (`requisitos.md` · origen: `desarrollador.md`, `rectora.md`).

Sin embargo, el discovery no aporta evidencia de que la infraestructura actual de la institución soporte Docker: no hay mención de servidor con motor de contenedores, pipeline de CI/CD ni experiencia del equipo en orquestación de contenedores. El PO registró este punto como pregunta abierta OQ-05 con la nota "requiere decisión arquitectónica (ADR) antes de comprometerse como historia" (`backlog.json` · OQ-05). El equipo de dos desarrolladores ya enfrenta la presión de entregar los reportes del período 2025-2026 para el cierre inminente (`mvp-canvas.md` · riesgos·2, `personas.md` · Rectora · `pain:documentos-urgentes-sin-entregar`).

## Decisión

El módulo de reportes académicos del MVP se desplegará con el mecanismo de despliegue existente en la institución (que el discovery describe implícitamente como un sistema .NET ya en funcionamiento). La containerización con Docker Compose se aplaza al siguiente ciclo de planificación, condicionado a que se confirmen: (a) la disponibilidad de infraestructura con soporte Docker en la institución, (b) la experiencia o capacidad del equipo para operarla, y (c) que no comprometa el plazo del cierre de período.

## Alternativas consideradas

- **Adoptar Docker Compose en el MVP actual** — Añade complejidad de infraestructura nueva (configuración de imágenes, volúmenes, redes) sin evidencia de que el entorno lo soporte; el riesgo de bloquear la entrega de los reportes urgentes es alto y no hay respaldo en el discovery de que el equipo lo haya operado antes. Violaría el principio de "lo más simple que funcione".
- **Desplegar solo el módulo de BD en contenedor y la capa de aplicación en el despliegue existente** — Solución híbrida que introduce complejidad de conectividad sin los beneficios completos de la containerización; tampoco hay evidencia de que la infraestructura lo soporte.

## Consecuencias

**Ganamos:** el equipo mantiene el foco en las funcionalidades que entregan valor directo al cierre del período 2025-2026; se evita un riesgo de infraestructura sin respaldo de evidencia.

**Aceptamos:** la deuda técnica de despliegue sin containerización persiste; el módulo heredará el proceso de despliegue actual, cuya mantenibilidad a largo plazo es menor. Esta decisión debe revisarse al inicio del siguiente ciclo: el desarrollador o la rectora deben confirmar si la infraestructura institucional soporta Docker antes de comprometer R-13 como historia.

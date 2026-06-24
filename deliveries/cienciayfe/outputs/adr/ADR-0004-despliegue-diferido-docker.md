# ADR-0004 — Despliegue diferido de Docker / contenedores

- **Estado:** Aceptado
- **Fecha:** 2026-06-24
- **Autores:** Architect · Agile Delivery Team

## Contexto

El sistema del Colegio Ciencia y Fe es una aplicación WinForms/.NET que se
despliega hoy en los equipos de escritorio del colegio. El MVP Canvas documenta
explícitamente que la unificación del sistema interno y el externo (`req:R-09`)
está fuera del alcance del MVP: "se aborda solo tras validar que los reportes son
correctos; derribar el sistema externo antes bloquea la operación".

No existe en el inbox ninguna evidencia de que el colegio cuente con
infraestructura de servidores, redes internas con Docker daemon, o experiencia
operativa del equipo en contenedores. El equipo tiene dos desarrolladores y el
ciclo 2025-2026 tiene una fecha de cierre fija.

El problema a resolver en el MVP es de calidad de datos y parametrización de
reportes, no de despliegue o escalabilidad.

## Decisión

No se conteneriza el sistema en el MVP. La aplicación WinForms continúa
desplegándose como hoy: instalación directa en los equipos del colegio, con SQL
Server en la instancia existente. No se introduce Docker, Kubernetes ni ninguna
capa de orquestación de contenedores.

La contenerización se evalúa en una iteración posterior, únicamente si el sistema
crece hacia un modelo multi-sede o si se decide integrar el sistema interno y el
externo (req:R-09, actualmente fuera de alcance).

## Alternativas consideradas

| Alternativa | Por qué se descartó |
|-------------|---------------------|
| Docker + SQL Server en contenedor local | Introduce complejidad de red, volúmenes y configuración de Docker en un entorno de escritorio educativo sin garantía de infraestructura. No aporta valor al MVP; el problema a resolver es de datos y reportes, no de despliegue. |
| SQL Server en contenedor + WinForms local | Separa la BD del equipo local sin necesidad clara. Agrega un punto de falla (red / daemon Docker) en un entorno donde la simplicidad es crítica para el equipo de dos personas. |
| Migración a aplicación web (ASP.NET / Blazor) | Cambia completamente la plataforma de entrega. Requiere rediseñar la UI, el modelo de despliegue y la integración con RDLC. Sobredimensionado y fuera del alcance del MVP; no hay evidencia en el inbox de que el colegio necesite acceso web. |

## Consecuencias

**Positivas:**
- El modelo de despliegue es conocido por el equipo y por el personal del colegio;
  no hay fricción de adopción.
- No se requiere infraestructura adicional (servidores, redes específicas, licencias
  de orquestación).
- El equipo dedica su capacidad al valor del MVP (esquema centralizado, plantillas
  parametrizadas) en lugar de a la operación de infraestructura.

**Negativas / riesgos:**
- Si el colegio crece a múltiples sedes y necesita una BD compartida entre sedes,
  el modelo de escritorio local mostrará sus límites. En ese momento se deberá
  evaluar la contenerización o una arquitectura cliente-servidor con SQL Server
  centralizado en red.
- Las actualizaciones de la aplicación siguen siendo manuales por equipo; no hay
  mecanismo de despliegue centralizado en el MVP. Este dolor es conocido y
  aceptado dado el tamaño del colegio.

## Trazabilidad

- Origina: MVP Canvas — fuera de alcance `req:R-09` · capacidad del equipo
  (dos desarrolladores, fecha límite fija)
- Impacta: ninguna historia del MVP directamente; decisión de no hacer que
  protege el foco del equipo

# ADR-0001 — Motor de reportes RDLC (mantener el existente)

- **Estado:** Aceptado
- **Fecha:** 2026-06-24
- **Autores:** Architect · Agile Delivery Team

## Contexto

El sistema de reportes del Colegio Ciencia y Fe es una aplicación WinForms/.NET
que ya utiliza Microsoft Report Viewer con plantillas RDLC para generar los
cuadros de calificaciones y el reporte de promociones. El dolor documentado en
`pain:rdlc-por-curso` no es causado por el motor en sí, sino por la forma en que
se usa: una plantilla RDLC separada por cada curso, con datos de materias quemados
en cada archivo. El requisito `req:R-06` exige plantillas parametrizadas y
reutilizables, no que se cambie el motor.

El equipo está formado por dos desarrolladores con conocimiento del stack actual
(.NET, SQL Server, RDLC). El ciclo escolar 2025-2026 tiene una fecha de cierre
fija que no admite una curva de aprendizaje sobre una nueva tecnología de reportes.

## Decisión

Se mantiene Microsoft Report Viewer / RDLC como motor de generación de reportes.
La deuda técnica se elimina cambiando *cómo* se usa RDLC —mediante plantillas
parametrizadas y datos leídos desde la BD centralizada— y no reemplazando el
motor. Esta es la opción más simple que resuelve el problema sin introducir riesgo
tecnológico en un ciclo con fecha límite fija.

## Alternativas consideradas

| Alternativa | Por qué se descartó |
|-------------|---------------------|
| Crystal Reports | Mismo nivel de vendor lock-in que RDLC. Migración costosa en tiempo y esfuerzo. No aporta valor diferencial para el problema a resolver. |
| FastReport | Requiere nueva licencia de pago y curva de aprendizaje para el equipo. El problema es de parametrización, no de capacidad del motor actual. |
| HTML + Puppeteer (PDF headless) | No encaja en el stack WinForms. Introduce un servidor Node.js o similar en un entorno de escritorio sin infraestructura adicional. Sobredimensionado para el problema. |

## Consecuencias

**Positivas:**
- Cero tiempo de migración de tecnología de reportes; el equipo puede centrarse
  en el valor: el esquema centralizado y la parametrización de plantillas.
- El conocimiento existente del equipo sobre RDLC se preserva y se profundiza.
- No se introduce ninguna nueva dependencia de terceros (licencias, servidores).

**Negativas / riesgos:**
- RDLC (Microsoft Report Viewer) tiene soporte oficial reducido en versiones
  modernas de .NET. Si el colegio migra a .NET 6+ en el futuro, se deberá
  evaluar la versión de la librería compatible o un reemplazo de motor. Este
  riesgo es externo al alcance del MVP 2025-2026.
- Las plantillas RDLC requieren Visual Studio o RDLC Designer para editarse; el
  desarrollador no puede modificarlas con un editor de texto plano.

## Trazabilidad

- Origina: `pain:rdlc-por-curso` · `req:R-06` · stack existente .NET/SQL Server
- Impacta: US-06 (plantilla parametrizada compartida)

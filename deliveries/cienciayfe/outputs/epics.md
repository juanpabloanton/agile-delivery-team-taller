# Épicas del MVP — Ciencia y Fe

> Producto: Módulo de reportes académicos dinámicos  
> Período objetivo: cierre 2025-2026  
> Fecha de generación: 2026-06-23  
> Fuente de verdad: `inbox/` (mvp-canvas.md, user-stories.md, requisitos.md, personas.md, evidence-map.json)

---

## E-01 · Esquema de datos centralizado de calificaciones

**Valor (outcome):** El desarrollador consulta calificaciones de cualquier curso desde una única fuente, eliminando la proliferación de stored procedures duplicados; la rectora obtiene información consistente en todos los reportes del período.  
**Prioridad:** 1 — Es el riesgo más alto del MVP (supuesto de migración del mvp-canvas) y es prerequisito de todo el motor de reportes. Sin datos centralizados, ninguna épica posterior puede entregar valor.  
**Origen:** `mvp:funcionalidades-minimas·1`, `mvp:riesgos·1`, `req:R-04`, `us:US-06`, `pain:informacion-no-centralizada`, `pain:sp-no-mantenibles`  
**Historias:** US-06, US-06a

---

## E-02 · Generación de reportes consistentes y sin devoluciones

**Valor (outcome):** La secretaria aprueba cuadros trimestrales, finales y reportes de promoción en el primer ciclo de revisión, sin devoluciones por inconsistencias de nombres de materias o datos que no coinciden entre documentos.  
**Prioridad:** 2 — Es el outcome central del MVP ("0 devoluciones por inconsistencias"). Depende del esquema centralizado (E-01) pero es el núcleo de valor para la secretaria y la rectora.  
**Origen:** `mvp:propuesta-valor`, `mvp:resultado-esperado`, `mvp:metrica-exito`, `req:R-01`, `req:R-02`, `us:US-01`, `us:US-02`, `us:US-07`, `pain:nombres-materias-quemados`, `pain:cambio-dana-otro-reporte`  
**Historias:** US-01, US-02, US-07

---

## E-03 · Plantilla única y mantenible de reportes

**Valor (outcome):** El desarrollador modifica plantillas en un único punto y el cambio se propaga a todos los cursos; la rectora ya no necesita actualizar archivo por archivo cuando el Ministerio cambia lineamientos.  
**Prioridad:** 3 — Habilita la mantenibilidad a largo plazo (R-11) y elimina el pain más costoso para el desarrollador (RDLC por curso). Depende de E-01 para los datos pero puede construirse en paralelo con E-02 desde el diseño.  
**Origen:** `mvp:funcionalidades-minimas·2`, `mvp:funcionalidades-minimas·3`, `req:R-03`, `req:R-11`, `us:US-05`, `pain:rdlc-por-curso`, `pain:plantillas-no-dinamicas`  
**Historias:** US-05

---

## E-04 · Formato de reporte conforme a requisitos del distrito

**Valor (outcome):** La secretaria puede enviar los reportes directamente al distrito sin ediciones manuales; la rectora cumple los compromisos formales de entrega del período 2025-2026.  
**Prioridad:** 4 — Agrega los elementos de cumplimiento y presentación que convierten un reporte técnicamente correcto en un documento aceptable por el distrito. Depende de E-02 y E-03.  
**Origen:** `mvp:funcionalidades-minimas·4`, `mvp:funcionalidades-minimas·5`, `req:R-05`, `req:R-06`, `req:R-10`, `us:US-03`, `us:US-04`, `us:US-08`, `pain:formato-reporte-inadecuado`, `pain:documentos-urgentes-sin-entregar`  
**Historias:** US-03, US-04, US-08

---

## Preguntas abiertas (no respaladas por el MVP o fuera de alcance)

| ID | Descripción | Origen | Motivo de exclusión |
|---|---|---|---|
| OQ-01 | R-07: Generación de nómina de abanderados 2026-2027 | `req:R-07`, `rectora.md` | Explícitamente fuera de alcance en el mvp-canvas ("prioridad siguiente, no del cierre actual") |
| OQ-02 | R-08: Consolidar sistema interno y externo | `req:R-08`, `rectora.md` | Explícitamente fuera de alcance en el mvp-canvas |
| OQ-03 | R-09: Mecanismo formal de comunicación de requerimientos secretaría→desarrollo | `req:R-09`, `secretaria.md`, `desarrollador.md` | No incluido en las funcionalidades mínimas del MVP; el pain `proceso-requerimientos-sin-claridad` es real pero la solución excede el módulo de reportes |
| OQ-04 | R-12: Cumplimiento Ley de Protección de Datos | `req:R-12` | El requisito existe pero el discovery no especifica qué datos se expondrían ni qué controles aplican; requiere análisis legal antes de definir historias |
| OQ-05 | R-13: Despliegue con Docker Compose | `req:R-13` | El discovery lo menciona como deseable pero no hay evidencia de que la infraestructura actual lo soporte; requiere decisión arquitectónica antes de comprometerse como historia |

---

## Diagrama del backlog

```mermaid
classDef epic fill:#2A9D8F,color:#fff,stroke:#1d7a6e,font-weight:bold;
classDef needs_refinement fill:#7C3AED,color:#fff,stroke:#5b2bb5;
classDef ready fill:#16A34A,color:#fff,stroke:#0e7a2e;

E01["E-01 · Esquema centralizado\nde calificaciones"]:::epic
E02["E-02 · Reportes consistentes\nsin devoluciones"]:::epic
E03["E-03 · Plantilla única\ny mantenible"]:::epic
E04["E-04 · Formato conforme\nal distrito"]:::epic

US06["US-06\nEsquema BD unificado"]:::needs_refinement
US06a["US-06a\nMigración de datos\nal esquema nuevo"]:::needs_refinement
US01["US-01\nNombres de materias\ndinámicos"]:::needs_refinement
US02["US-02\nReportes independientes\nentre sí"]:::needs_refinement
US07["US-07\nReportes listos antes\ndel cierre de período"]:::needs_refinement
US05["US-05\nPlantilla única\nconfigurable por curso"]:::needs_refinement
US03["US-03\nFirmas y sellos\nen el reporte"]:::needs_refinement
US04["US-04\nJerarquía visual\nde escalas"]:::needs_refinement
US08["US-08\nEscalas por nivel\neducativo automáticas"]:::needs_refinement

E01 --> US06
E01 --> US06a
E02 --> US01
E02 --> US02
E02 --> US07
E03 --> US05
E04 --> US03
E04 --> US04
E04 --> US08

US06 --> US01
US06 --> US05
US01 --> US02
US05 --> US03
US05 --> US04
US05 --> US08
```

> Todas las historias están marcadas en morado (necesitan refinamiento por el Developer). Ninguna alcanza verde hasta pasar el gate DoR/INVEST.

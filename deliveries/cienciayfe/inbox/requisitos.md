# Requisitos candidatos — Ciencia y Fe

## Funcionales

- **[R-01]** El sistema debe generar cuadros de calificaciones trimestrales y quimestrales de forma dinámica para todos los cursos, con nombres de materias obtenidos de la base de datos (no quemados en plantillas).
  - Tipo: funcional
  - Origen: `secretaria.md`, `rectora.md` · Secretaria, Rectora

- **[R-02]** El sistema debe generar cuadros finales de calificaciones de forma dinámica, coherentes con los cuadros trimestrales del mismo período.
  - Tipo: funcional
  - Origen: `secretaria.md`, `rectora.md` · Secretaria, Rectora

- **[R-03]** El sistema debe generar reportes de promociones cuyos nombres de materias coincidan exactamente con los del cuadro de calificación y provengan de la base de datos (sin llenado por posicionamiento).
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

- **[R-04]** Los reportes deben incluir espacio reservado para la firma del docente y dos sellos (colegio y distrito).
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

- **[R-05]** El sistema debe soportar escalas de calificación diferenciadas por nivel: cualitativa (+A / -A / B-) para inicial y básica elemental (hasta 2.º), y cuantitativa (numérica) para básica media en adelante (desde 3.º); con escalas de comportamiento configurables por nivel.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

- **[R-06]** El sistema debe usar plantillas de reporte parametrizadas y reutilizables entre cursos; no una plantilla RDLC o Word separada por curso.
  - Tipo: funcional
  - Origen: `desarrollador.md`, `rectora.md` · Desarrollador, Rectora

- **[R-07]** La base de datos debe centralizar las calificaciones en un esquema unificado y coherente que sirva a todos los cursos y tipos de reporte.
  - Tipo: funcional
  - Origen: `rectora.md`, `desarrollador.md` · Rectora, Desarrollador

- **[R-08]** El sistema debe generar la nómina de estudiantes abanderados para el período 2026-2027.
  - Tipo: funcional
  - Origen: `rectora.md` · Rectora

- **[R-09]** El sistema debe unificar los sistemas interno y externo actuales en una sola plataforma.
  - Tipo: funcional
  - Origen: `rectora.md` · Rectora

- **[R-10]** Debe existir un flujo documentado para que secretaría solicite, especifique y valide cambios en reportes antes de que desarrollo los implemente.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

## No funcionales

- **[R-11]** El sistema debe cumplir con la Ley de Protección de Datos Personales vigente en Ecuador.
  - Tipo: no funcional
  - Origen: `rectora.md` · Rectora

- **[R-12]** El código y los SP del sistema deben ser mantenibles: sin lógica de posicionamiento, sin materias quemadas, sin SP de más de un nivel de anidación razonable, y con responsabilidades separadas.
  - Tipo: no funcional
  - Origen: `desarrollador.md`, `rectora.md` · Desarrollador, Rectora

- **[R-13]** El sistema debe adaptarse a cambios en los lineamientos académicos del Ministerio de Educación mediante configuración, sin requerir modificaciones manuales archivo por archivo.
  - Tipo: no funcional
  - Origen: `rectora.md` · Rectora

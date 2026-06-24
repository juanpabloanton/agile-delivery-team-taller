# Requisitos candidatos — Ciencia y Fe

Todos los requisitos derivan exclusivamente de evidencia en las entrevistas de este discovery.

---

## Funcionales

- **[R-01]** El sistema debe generar cuadros de calificaciones trimestrales y finales leyendo los nombres de las materias dinámicamente desde la base de datos, sin valores quemados en la plantilla.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria / `desarrollador.md` · Desarrollador

- **[R-02]** El sistema debe garantizar que los nombres de las materias en el cuadro final coincidan automáticamente con los del cuadro trimestral y con el reporte de promoción.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

- **[R-03]** El sistema debe proveer una plantilla única de reporte (cuadro trimestral, final y promoción) reutilizable y configurable por curso, sin necesidad de duplicar archivos RDLC o Word por nivel.
  - Tipo: funcional
  - Origen: `desarrollador.md` · Desarrollador / `rectora.md` · Rectora

- **[R-04]** El sistema debe implementar un esquema de base de datos centralizado para calificaciones, con una sola tabla unificada que sirva a todos los cursos y reportes.
  - Tipo: funcional
  - Origen: `rectora.md` · Rectora / `desarrollador.md` · Desarrollador

- **[R-05]** El sistema debe diferenciar las escalas de calificación por nivel educativo: cualitativa en inicial, simbólica (+A / -A / B−) hasta segundo grado, y cuantitativa de tercero en adelante; y debe aplicar las escalas de comportamiento correspondientes a cada nivel.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

- **[R-06]** Los reportes de cuadros y promociones deben incluir espacios designados para la firma del docente y dos sellos (colegio y distrito).
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria / `rectora.md` · Rectora

- **[R-07]** El sistema debe generar la nómina de estudiantes abanderados para el período 2026-2027.
  - Tipo: funcional
  - Origen: `rectora.md` · Rectora

- **[R-08]** El sistema debe consolidar los dos sistemas existentes (interno y externo) en uno solo, eliminando la duplicidad de mantenimiento.
  - Tipo: funcional
  - Origen: `rectora.md` · Rectora

- **[R-09]** El sistema debe proveer un mecanismo formal (flujo de aprobación o especificación estructurada) para que secretaría comunique requerimientos de cambio en reportes al equipo de desarrollo.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria / `desarrollador.md` · Desarrollador

- **[R-10]** Los cuadros de comportamiento y sus escalas informativas deben mostrarse con una jerarquía visual menor que los datos de calificaciones, de modo que no ocupen espacio excesivo en el reporte.
  - Tipo: funcional
  - Origen: `secretaria.md` · Secretaria

---

## No funcionales

- **[R-11]** El sistema debe ser mantenible: cualquier cambio en plantillas, nombres de materias o escalas debe realizarse en un único punto y propagarse a todos los reportes (principio DRY).
  - Tipo: no funcional
  - Origen: `desarrollador.md` · Desarrollador / `rectora.md` · Rectora

- **[R-12]** El sistema debe cumplir la Ley de Protección de Datos Personales vigente en la jurisdicción de la institución.
  - Tipo: no funcional
  - Origen: `rectora.md` · Rectora

- **[R-13]** Los módulos del sistema nuevo deben desplegarse como contenedores Docker con un archivo `docker-compose` que permita levantar el ecosistema completo en un solo comando.
  - Tipo: no funcional
  - Origen: `desarrollador.md` · Desarrollador / `rectora.md` · Rectora

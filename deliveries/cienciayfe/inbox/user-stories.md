# User Stories — Ciencia y Fe

Las historias cubren el **núcleo de valor** del MVP: generación correcta de los
documentos de cierre académico 2025-2026 a partir de una fuente de datos
unificada. Se priorizan los dolores que aparecen en múltiples entrevistas y
bloquean la entrega al distrito.

---

- **[US-01]** Como Secretaria, quiero generar el cuadro de calificaciones
  trimestral de cualquier curso con los nombres de materias obtenidos de la base
  de datos, para enviarlo al distrito sin errores de nomenclatura.
  - Criterios de aceptación:
    - Dado un curso y un trimestre, cuando genero el cuadro, entonces los nombres
      de materias coinciden exactamente con los registrados en la BD (no provienen
      de texto quemado en la plantilla).
    - Dado que se actualiza el nombre de una materia en la BD, cuando regenero el
      cuadro, entonces el nuevo nombre aparece sin modificar ninguna plantilla ni
      SP.
  - Fuente: `secretaria.md`

---

- **[US-02]** Como Secretaria, quiero generar el cuadro de calificaciones final
  que sea coherente con los cuadros trimestrales del mismo período, para
  entregar documentos consistentes al distrito.
  - Criterios de aceptación:
    - Dado un curso y un período, cuando genero el cuadro final, entonces los
      promedios coinciden con los valores registrados en los cuadros trimestrales
      de ese período.
    - Dado una inconsistencia en la BD, cuando intento generar el cuadro final,
      entonces el sistema alerta del conflicto antes de producir el documento.
  - Fuente: `secretaria.md`, `rectora.md`

---

- **[US-03]** Como Secretaria, quiero generar el reporte de promociones con los
  nombres de materias provenientes de la base de datos y coincidentes con los del
  cuadro de calificaciones, para evitar inconsistencias entre documentos
  entregados al distrito.
  - Criterios de aceptación:
    - Dado un curso, cuando genero el reporte de promociones, entonces cada
      materia listada comparte el mismo identificador y nombre que en el cuadro de
      calificaciones de ese curso.
    - Dado el reporte generado, cuando lo reviso, entonces no existe llenado por
      posicionamiento: cada celda se llena por referencia a su materia en la BD.
  - Fuente: `secretaria.md`

---

- **[US-04]** Como Secretaria, quiero que los reportes incluyan espacio
  reservado para la firma del docente y los sellos del colegio y del distrito,
  para que los documentos cumplan los requisitos formales de entrega.
  - Criterios de aceptación:
    - Dado cualquier tipo de reporte impreso (calificaciones o promociones),
      cuando lo genero, entonces el documento incluye el espacio de firma del
      docente, el sello del colegio y el sello del distrito en posiciones
      estándar.
  - Fuente: `secretaria.md`

---

- **[US-05]** Como Secretaria, quiero que el sistema aplique automáticamente la
  escala de calificación correcta según el nivel del curso, para que los reportes
  cumplan los lineamientos del Ministerio de Educación.
  - Criterios de aceptación:
    - Dado un estudiante de inicial o básica elemental (hasta 2.° grado), cuando
      genero su cuadro, entonces las calificaciones aparecen en escala cualitativa
      (+A / -A / B-).
    - Dado un estudiante de básica media en adelante (desde 3.° grado), cuando
      genero su cuadro, entonces las calificaciones aparecen en escala numérica.
  - Fuente: `secretaria.md`

---

- **[US-06]** Como Desarrollador, quiero usar una plantilla de reporte
  parametrizada compartida por todos los cursos, para que un cambio de diseño o
  estructura aplique a todos sin tocar archivos individuales.
  - Criterios de aceptación:
    - Dado un cambio en un elemento de diseño (logo, encabezado), cuando
      actualizo la plantilla única, entonces todos los reportes de todos los
      cursos reflejan el cambio en la siguiente generación.
    - Dado el registro de un nuevo curso en la BD, cuando solicito su reporte,
      entonces el sistema lo genera con la plantilla compartida sin crear un
      template adicional.
  - Fuente: `desarrollador.md`, `rectora.md`

---

- **[US-07]** Como Desarrollador, quiero que las calificaciones estén en un
  esquema unificado de la BD con identificadores de materia, curso y período,
  para que todos los reportes lean de la misma fuente y sean coherentes entre sí.
  - Criterios de aceptación:
    - Dado un registro de calificación en el esquema centralizado, cuando lo
      consulto desde cualquier módulo de reporte, entonces el valor y el nombre de
      materia son idénticos en todos los documentos del mismo período.
    - Dado el nuevo esquema, cuando agrego un nuevo tipo de reporte, entonces no
      se requiere duplicar tablas de calificaciones.
  - Fuente: `rectora.md`, `desarrollador.md`

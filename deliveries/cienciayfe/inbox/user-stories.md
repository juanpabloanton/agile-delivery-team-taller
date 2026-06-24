# User Stories — Ciencia y Fe

Historias priorizadas por el núcleo de valor: generar reportes académicos
correctos y consistentes para el cierre del período 2025-2026.

---

## Secretaria

- **[US-01]** Como secretaria, quiero que los cuadros de calificaciones muestren
  los nombres de las materias leídos dinámicamente desde la base de datos, para
  no tener que detectar ni corregir inconsistencias de nombres manualmente.
  - Criterios de aceptación:
    - Dado que se genera un cuadro trimestral, cuando el sistema lo produce,
      entonces los nombres de las materias coinciden exactamente con los
      registrados en la BD, sin valores hardcoded en la plantilla.
    - Dado que un nombre de materia cambia en la BD, cuando se regenera el
      reporte, entonces el cambio se refleja sin tocar la plantilla.
  - Fuente: `secretaria.md`, `desarrollador.md`

- **[US-02]** Como secretaria, quiero que corregir el layout o los datos de un
  reporte no altere los demás reportes del mismo período, para poder revisar cada
  documento de forma independiente.
  - Criterios de aceptación:
    - Dado que se corrige el cuadro trimestral, cuando se regenera, entonces el
      reporte de promoción y el cuadro final mantienen su layout sin cambios.
    - Dado que se actualiza el cuadro final, cuando se comparan con el reporte de
      promoción, entonces los nombres de materias y valores coinciden en ambos.
  - Fuente: `secretaria.md`

- **[US-03]** Como secretaria, quiero que los cuadros y reportes de promoción
  incluyan espacios designados para la firma del docente y dos sellos (colegio y
  distrito), para poder enviarlos al distrito sin modificaciones manuales en el
  documento.
  - Criterios de aceptación:
    - Dado que se genera cualquier reporte, cuando se visualiza o imprime, entonces
      aparecen los espacios de firma del docente y los dos sellos (colegio,
      distrito) en la posición definida por el formato institucional.
    - Dado que el reporte contiene calificaciones de segundo a bachillerato, cuando
      se envía al distrito, entonces el formato cumple los requisitos del distrito
      sin edición adicional.
  - Fuente: `secretaria.md`, `rectora.md`

- **[US-04]** Como secretaria, quiero que las escalas de calificación y de
  comportamiento se muestren con jerarquía visual menor que los datos de
  calificaciones, para que la información principal sea fácil de leer en el
  reporte impreso.
  - Criterios de aceptación:
    - Dado que se genera un cuadro, cuando se imprime, entonces la sección de
      calificaciones ocupa más espacio visual que la sección de escalas de
      comportamiento.
    - Dado que el reporte es de nivel inicial o básica inferior, cuando se
      visualiza, entonces la escala cualitativa/simbólica aparece en tamaño
      secundario, no dominante.
  - Fuente: `secretaria.md`

---

## Desarrollador

- **[US-05]** Como desarrollador, quiero una plantilla de reporte única y
  configurable por nivel/curso, para que cuando el Ministerio cambie los
  lineamientos solo tenga que actualizar un archivo y el cambio se propague a
  todos los cursos.
  - Criterios de aceptación:
    - Dado que existen N cursos, cuando se genera cualquier reporte, entonces
      todos comparten la misma plantilla base con parámetros por curso (no RDLC
      separados por curso).
    - Dado que se modifica la configuración de una materia en un curso, cuando se
      regeneran los reportes de ese curso, entonces el cambio se refleja sin tocar
      plantillas de otros cursos.
  - Fuente: `desarrollador.md`, `rectora.md`

- **[US-06]** Como desarrollador, quiero un esquema de base de datos centralizado
  para calificaciones con una tabla unificada, para poder escribir una sola lógica
  de consulta que sirva a todos los reportes sin duplicar stored procedures.
  - Criterios de aceptación:
    - Dado que se consultan calificaciones de cualquier curso, cuando se ejecuta
      la consulta, entonces proviene de la misma tabla/vista unificada.
    - Dado que se agrega un nuevo tipo de reporte, cuando se implementa, entonces
      reutiliza la lógica de consulta existente sin crear un nuevo SP.
  - Fuente: `desarrollador.md`, `rectora.md`

---

## Rectora

- **[US-07]** Como rectora, quiero que los cuadros trimestrales, finales y
  reportes de promoción del período 2025-2026 estén disponibles y correctos antes
  del cierre de período, para cumplir los compromisos con el distrito a tiempo.
  - Criterios de aceptación:
    - Dado que es el cierre del período 2025-2026, cuando la secretaria revisa los
      reportes generados, entonces los aprueba sin devoluciones por inconsistencias
      de nombres o layout roto.
    - Dado que los reportes de segundo a bachillerato son los críticos, cuando se
      generan, entonces están listos antes que los de inicial y básica inferior.
  - Fuente: `rectora.md`, `secretaria.md`

- **[US-08]** Como rectora, quiero que la lógica de calificaciones aplique
  automáticamente la escala correcta según el nivel educativo (cualitativa en
  inicial, simbólica hasta segundo, cuantitativa de tercero en adelante), para
  no requerir ajustes manuales al cambiar de nivel.
  - Criterios de aceptación:
    - Dado que se genera un cuadro de inicial, cuando se visualiza, entonces
      muestra escalas cualitativas sin calificaciones numéricas.
    - Dado que se genera un cuadro de básica superior o bachillerato, cuando se
      visualiza, entonces muestra calificaciones numéricas en la escala definida.
    - Dado que se genera un cuadro de primero o segundo de básica, cuando se
      visualiza, entonces muestra la escala simbólica (+A / −A / B−) definida.
  - Fuente: `secretaria.md`, `rectora.md`

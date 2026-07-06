# Informe final 2/3: Agile Delivery Team — Ciencia y Fe

## Resumen

El Agile Delivery Team recibe los artefactos del Discovery Agent y los transforma en
un plan de entrega ejecutable: épicas, historias refinadas, arquitectura, ADRs y un
Sprint Plan. Ocupa la segunda posición del flujo:

```text
Discovery Agent  ->  Agile Delivery Team  ->  Quality Agent
evidencia validada    trabajo listo           código verificado
```

El caso utilizado es **Ciencia y Fe**. El equipo no inventa necesidades nuevas: cada
decisión debe conservar trazabilidad hacia los archivos ubicados en
`deliveries/cienciayfe/inbox/`.

## Objetivo y alcance

El objetivo fue convertir historias iniciales en unidades pequeñas, estimadas,
testeables y priorizadas. Esta etapa define cómo construir la solución, pero no
declara que el código esté aprobado; esa decisión corresponde al Quality Agent.

## Entrada procedente de Discovery

```text
deliveries/cienciayfe/inbox/
├── personas.md
├── requisitos.md
├── user-stories.md
├── mvp-canvas.md
└── evidence-map.json
```

Este `inbox/` es el contrato entre proyectos. Por ejemplo, la historia US-01 y el
requisito R-01 conservan como origen el dolor de materias quemadas identificado en
la entrevista de la Secretaria.

## Prompts y comandos utilizados

```text
/delivery:generate-epics deliveries/cienciayfe
/delivery:generate-stories deliveries/cienciayfe
/delivery:architecture deliveries/cienciayfe
/delivery:sprint-plan deliveries/cienciayfe 20
/delivery:report deliveries/cienciayfe
```

| Orden | Comando / agente | Motivo | Resultado |
|---:|---|---|---|
| 1 | `generate-epics` / Product Owner | Agrupar historias y priorizar valor. | `epics.md`, `backlog.json` |
| 2 | `generate-stories` / Developer | Refinar Gherkin, dependencias y puntos. | `stories.md`, backlog actualizado |
| 3 | `architecture` / Architect | Definir componentes y justificar decisiones. | `architecture.md`, `adr/*.md` |
| 4 | `sprint-plan` / Scrum Master | Comprometer solo historias listas dentro de 20 puntos. | `sprint-plan.md`, `sprint-plan.json` |
| 5 | `report` | Construir una vista HTML determinista. | `report.html` |

Cada comando está definido en `.claude/commands/delivery/`. Product Owner,
Developer, Architect y Scrum Master son agentes especializados; el gate, y no una
opinión del modelo, decide si las historias están listas.

## Análisis de la ejecución

### 1. Prueba negativa: historia deliberadamente defectuosa

Se agregó **US-99** al backlog con defectos intencionales: descripción vaga, sin
criterios de aceptación, 13 puntos, dependencia inexistente y preguntas abiertas.

![Historia problemática agregada](<deliveries/evidencia/1.-se agrego historia floja.png>)

### 2. Prompt y detección del problema

Se ejecutó:

```text
/delivery:generate-stories deliveries/cienciayfe
```

La captura muestra que el orquestador leyó el backlog y la skill, identificó US-99
y lanzó al subagente Developer para refinar las historias.

![El gate detecta US-99](<deliveries/evidencia/2.-genero bloqueo por historia problematica es la que agregamos.png>)

Log visible:

```text
US-99: vaga, sin AC, 13 pts, depende de US-404 inexistente, preguntas abiertas
Lanzando el subagente developer para refinar todas las historias.
```

### 3. Corrección y finalización

El gate impidió avanzar mientras existían historias no testeables. El Developer
debió agregar criterios Gherkin, resolver dependencias, ajustar estimaciones o
dividir historias mayores de 8 puntos.

![Corrección durante el bloqueo](<deliveries/evidencia/3.-durante el bloqueo.png>)

![Ejecución finalizada](<deliveries/evidencia/4.-fin de la ejecucion.png>)

## Resultado de la historia US-01

La historia originada en Discovery quedó refinada como:

- **Título:** Cuadro de calificaciones trimestral con nombres desde la BD.
- **Estimación:** 5 puntos.
- **Prioridad:** 2.
- **Dependencia:** US-07, esquema unificado de calificaciones.
- **Trazabilidad:** `us:US-01`, `req:R-01`, `pain:materias-quemadas` y
  `pain:documentos-urgentes-cierre-ano`.
- **Pruebas esperadas:** nombres obtenidos de la BD, propagación de cambios y
  presencia de todos los estudiantes del curso.

El archivo `stories.md` registra **7 historias listas y 34 puntos totales**. Esto no
significa que las 34 unidades entren en un Sprint de 20 puntos; el Scrum Master debe
seleccionar por prioridad y capacidad.

## Resultados y evidencias

| Evidencia | Ubicación | Qué demuestra |
|---|---|---|
| Entrada de Discovery | `deliveries/cienciayfe/inbox/` | Origen de producto |
| Épicas | `outputs/epics.md` | Agrupación por valor |
| Backlog | `outputs/backlog.json` | Prioridad, origen y estado INVEST |
| Historias | `outputs/stories.md` | Gherkin, puntos y dependencias |
| Arquitectura | `outputs/architecture.md` | Componentes y decisiones |
| ADRs | `outputs/adr/` | Motivos y alternativas técnicas |
| Sprint | `outputs/sprint-plan.json` | Compromiso dentro de capacidad |

## Gate DoR/INVEST

`.claude/hooks/dor-invest-gate.py` custodia `stories.md` y el Sprint Plan. Bloquea
historias que no tengan formato completo, criterios, estimación válida, dependencias
resueltas o tamaño manejable. Si devuelve `exit 2`, se corrige el backlog y se
reintenta el mismo comando; no se evade escribiendo otro archivo.

## Entrega a Quality

Después de implementar una historia, sus criterios Gherkin deben expresarse en un
`specs/<feature>/spec.md` con requisitos `FR-xxx`. Quality recibe:

```text
historia refinada + criterios -> spec.md -> código + pruebas
```

El identificador de origen debe mantenerse para poder recorrer la cadena inversa:

```text
prueba -> FR -> criterio Gherkin -> US -> requisito -> entrevista
```

## Limitación verificable

Las evidencias de este repositorio terminan en el plan de Ciencia y Fe. El repositorio
Quality contiene una ejecución completa sobre **Citas Salud**, no sobre una
implementación de Ciencia y Fe. Por tanto, la integración está demostrada mediante
contratos de artefactos, pero falta ejecutar Quality sobre la misma US-01 para cerrar
una demostración de extremo a extremo.

## Conclusión

El Agile Delivery Team convirtió evidencia de negocio en historias implementables y
demostró que su gate rechaza trabajo defectuoso. La salida entrega al desarrollo una
definición clara y a Quality criterios observables con los que decidir si la solución
realmente cumple.

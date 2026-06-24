# agile-delivery-team

Convierte los artefactos de discovery de un delivery en un plan de entrega: épicas, historias refinadas y arquitectura con ADRs.

## Flujo de trabajo

Ejemplo de delivery actual: `deliveries/cienciayfe`.

| Fase | Claude Code | Resultado |
|---|---|---|
| Épicas y backlog | `/delivery:generate-epics deliveries/cienciayfe` | `outputs/epics.md`, `outputs/backlog.json` |
| Historias listas | `/delivery:generate-stories deliveries/cienciayfe` | `outputs/stories.md`, `outputs/backlog.json` actualizado |
| Arquitectura | `/delivery:architecture deliveries/cienciayfe` | `outputs/architecture.md`, `outputs/adr/` |
| Plan de Sprint | `/delivery:sprint-plan deliveries/cienciayfe` | `outputs/sprint-plan.md`, `outputs/sprint-plan.json` |
| Reporte | `/delivery:report deliveries/cienciayfe` | `report.html` |

Ejecuta los comandos desde la raíz de `agile-delivery-team`. Cada delivery requiere sus entradas de discovery en `deliveries/<nombre>/inbox/`.

## Uso desde Codex

Codex no registra automáticamente los comandos slash definidos en `.claude/`. Usa estos cuatro prompts, en orden:

### 1. Épicas y backlog

```text
Lee agile-delivery-team/.claude/commands/delivery/generate-epics.md y ejecuta el flujo para agile-delivery-team/deliveries/cienciayfe, usando exclusivamente las entradas de su inbox.
```

### 2. Historias refinadas

```text
Lee agile-delivery-team/.claude/commands/delivery/generate-stories.md y ejecuta el flujo para agile-delivery-team/deliveries/cienciayfe usando su backlog existente. Actualiza backlog.json y genera stories.md solo si las historias cumplen INVEST y Definition of Ready.
```

### 3. Arquitectura y ADRs

```text
Lee agile-delivery-team/.claude/commands/delivery/architecture.md y ejecuta el flujo para agile-delivery-team/deliveries/cienciayfe. Genera architecture.md y los ADRs trazables necesarios, sin decidir tecnologías no respaldadas por el discovery.
```

### 4. Plan de Sprint

```text
Lee agile-delivery-team/.claude/commands/delivery/sprint-plan.md y ejecuta el flujo para agile-delivery-team/deliveries/cienciayfe usando su backlog refinado. Aplica la capacidad predeterminada de 20 puntos y genera sprint-plan.md y sprint-plan.json.
```

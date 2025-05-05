# Sistema de Efectos

La clase [`Effect`](../../../Assets/src/app/Effects/Effects.cs) es una clase abstracta que representa un efecto en un sistema de juego, heredando de `Entity`. Esta clase proporciona la funcionalidad base para manejar efectos que pueden aplicarse a unidades, incluyendo características como duración, acumulación de pilas y efectos visuales.

# Características Principales

| Característica | Descripción |
| --- | --- |
| Sistema de Pilas | Permite acumular múltiples instancias del mismo efecto |
| Duración | Controla cuándo se elimina el efecto (por turnos o combate) |
| Efectos Visuales | Implementa sistema de partículas para efectos de aplicación |
| Propietarios | Mantiene referencias al propietario y lanzador del efecto |

## Propiedades Principales

### stacks

- Tipo: int
- Descripción: Número de pilas acumuladas del efecto
- Uso: Controla la intensidad o cantidad de aplicaciones del efecto

### owner

- Tipo: Unit
- Descripción: Unidad que posee el efecto
- Uso: Referencia al propietario actual del efecto

### thrower

- Tipo: Unit
- Descripción: Unidad que aplicó el efecto
- Uso: Referencia al lanzador original del efecto

## Configuración Visual

### showIconOnApply

- Tipo: bool
- Valor por defecto: true
- Descripción: Controla si se muestra el ícono cuando se aplica el efecto

### burstPosition

- Tipo: PivotPosition
- Descripción: Define dónde se muestra el efecto visual
- Valores posibles: center, top, bottom, left, right

### randomBurst

- Tipo: bool
- Descripción: Determina si el efecto visual aparece en posición aleatoria

## Métodos de Aplicación

### Apply(Unit unit)

- Aplica el efecto a una unidad específica
- Actualiza la visualización según las configuraciones
- Notifica a los sistemas relacionados

### Apply(List<Unit> units)

- Aplica el efecto a múltiples unidades simultáneamente
- Necesario para efectos especificos de área o cadena

### Remove()

- Elimina el efecto de su unidad objetivo
- Restaura el estado de la unidad sin este efecto

## Ciclo de Vida

### OnStartTurn()

- Se ejecuta al inicio de cada turno
- Retorna booleano indicando si necesita tiempo de ejecución
- Verifica condiciones de eliminación temporal

### OnEndTurn()

- Se ejecuta al finalizar cada turno
- Gestiona la duración del efecto
- Actualiza el contador de turnos restantes

### OnEndCombat()

- Se ejecuta al finalizar el combate
- Elimina el efecto si está configurado para ello

# Configuración Avanzada

## Configuración de Duración

### onTimeForceRemove

- Controla cuándo se elimina el efecto automáticamente
- Opciones: none, EndTurn, StartTurn

### turnsDurationToRemove

- Número de turnos antes de la eliminación automática
- Valor de 0 desactiva la eliminación temporal

## Sistema de Pilas

### acumulate

- Permite o impide la acumulación de múltiples instancias
- Valor por defecto: true

### ForceStacks(int stacks)

- Fuerza un número específico de pilas
- Útil para efectos que necesitan una intensidad exacta

# Notas Importantes

<!-- - La clase utiliza el patrón de diseño Factory mediante los métodos Instantiate -->
<!-- - Implementa un sistema de eventos para notificar cambios en el estado del efecto -->
- Todos los efectos visuales son opcionales y configurables
- El sistema de duración es flexible y puede ser controlado manualmente

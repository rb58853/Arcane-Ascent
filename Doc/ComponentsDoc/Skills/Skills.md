# Sistema de Habilidades

La clase [`Skill`](../../../Assets/src/app/Skills/Skills.cs) es una clase abstracta que representa una habilidad en un sistema de juego, heredando de Entity e implementando [`IIcon`](../../../Assets/src/app/Actions/IIcon.cs). Esta clase proporciona la funcionalidad base para manejar habilidades que pueden ser ejecutadas por unidades o cartas, incluyendo características como tipos de lanzamiento, objetivos automáticos y efectos visuales.

# Características Principales

| Característica | Descripción |
| --- | --- |
| Tipos de Lanzamiento | Controla cómo se ejecuta la habilidad (auto, target, multitarget) |
| Objetivos Automáticos | Define quién puede ser objetivo de la habilidad |
| Momentos de Activación | Controla cuándo se activa la habilidad automáticamente |
| Efectos Visuales | Maneja la representación visual de la habilidad |
| Sistema de Pilas | Controla la cantidad de veces que se puede ejecutar |

# Referencia de Propiedades

## Propiedades Principales

### autoTarget

- Tipo: `AutoTarget`
- Descripción: Define el objetivo automático de la habilidad
- Valores posibles:
  - `ownerAutotarget`: El propietario de la habilidad
  - `self`: La unidad misma
  - `allies`: Las unidades aliadas
  - `enemies`: Las unidades enemigas
  - `enemieHero`: El héroe enemigo
  - `randomEnemie`: Un enemigo aleatorio
  - `randomAllie`: Un aliado aleatorio
  - `fullAlliesTeam`: Todo el equipo aliado
  - `onTargetEnemie`: Un objetivo enemigo específico
  - `All`: Todas las unidades
  - `SummonOwner`: El propietario de la invocación
  - `SummonedAllies`: Las invocaciones aliadas
  - `Target`: Indica que requiere un objetivo (solo para lanzamiento)

### onTimeActivation

- Tipo: `OnTime`
- Descripción: Momento de activación automática de la habilidad
- Valores posibles:
  - `Active`: Durante la ejecución
  - `TakeDamage`: Al recibir daño
  - `ApplyDamage`: Al aplicar daño
  - `EndAttack`: Al finalizar un ataque
  - `StartCombat`: Al comenzar el combate
  - `StartTurn`: Al inicio del turno
  - `EndTurn`: Al final del turno
  - `Initialize`: Al inicializar
  - `Death`: Al morir
  - `Health`: Relacionado con la salud
  - `ThrowAttack`: Al lanzar un ataque
  - `Setter`: Al establecer un valor
  - `Invoke`: Al invocar
  - `breakShield`: Al romper escudo
  - `beforeStartTurn`: Antes de comenzar el turno

### launchRate

- Tipo: `float`
- Descripción: Velocidad de lanzamiento de la habilidad
- Valor por defecto: `0.3f`

### maxEjecutionTimes

- Tipo: `int`
- Descripción: Número máximo de veces que se puede ejecutar la habilidad
- Valor por defecto: `-1` (sin límite)

### throwType

- Tipo: `ThrowType`
- Descripción: Tipo de lanzamiento de la habilidad
- Valores posibles: melee, ranged

## Configuración Visual

### useAnim

- Tipo: `bool`
- Descripción: Controla si se usa animación al lanzar la habilidad
- Valor por defecto: `true`

### effectObject

- Tipo: EffectObject
- Descripción: Efecto visual asociado a la habilidad

### Icon

- Tipo: Sprite
- Descripción: Icono personalizado para la habilidad
- Nota: Si está vacío, usa el icono por defecto

# Métodos Principales

## Métodos de Ejecución

### Execute(Unit target)

- Ejecuta la habilidad sobre una unidad específica
- Método abstracto que debe ser implementado por las clases derivadas
- Retorna bool indicando si la ejecución fue exitosa

### Execute(List<Unit> targets)

- Ejecuta la habilidad sobre múltiples unidades simultáneamente
- Útil para habilidades de área o efectos en grupo
- Retorna bool indicando si la ejecución fue exitosa

### Execute()

- Ejecuta la habilidad automáticamente sobre los objetivos definidos
- Verifica el contador de ejecuciones antes de proceder
- Retorna bool indicando si la ejecución fue exitosa

## Métodos de Inicialización

### Initialize(Unit owner)

- Inicializa la habilidad con un propietario específico
- Se ejecuta automáticamente al asignar un propietario
- Verifica la condición de activación inicial

### Initialize(Card card)

- Inicializa la habilidad con una carta específica
- Conecta la habilidad con el héroe de la carta
- Inicializa el propietario automáticamente

## Métodos de Instanciación

### CreateInstance(Unit owner)

- Crea una nueva instancia de la habilidad
- Asigna el propietario especificado
- Retorna la nueva instancia clonada

### CreateInstance(Card card)

- Crea una nueva instancia de la habilidad
- Asigna la carta como propietaria
- Inicializa la habilidad automáticamente

# Notas Importantes

<!-- - La clase implementa el patrón de diseño Factory mediante los métodos CreateInstance -->
- El sistema de objetivos automáticos permite flexibilidad en la selección de targets
- Las habilidades pueden ser activas (requieren ejecución manual) o pasivas (se activan automáticamente)
- El sistema de efectos visuales es opcional y configurable
- La clase proporciona métodos virtuales para personalización de comportamiento

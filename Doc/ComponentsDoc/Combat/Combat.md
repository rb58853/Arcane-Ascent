# Sistema de Combate

La clase [`Combat`](../../../Assets/src/app/Combat/Combat.cs) representa un sistema de combate completo en el juego, manejando turnos, equipos y unidades en combate. Esta clase es la columna vertebral del sistema de combate, coordinando todas las interacciones entre unidades y equipos.

# Características Principales

| Característica | Descripción |
| --- | --- |
| Gestión de Equipos | Maneja dos equipos: aliados y enemigos |
| Sistema de Turnos | Controla el flujo de turnos entre unidades |
| Persistencia | Mantiene datos importantes durante la ejecución |
| Efectos de Combate | Gestiona efectos que se aplican al inicio del combate |

# Referencia de Propiedades

## Propiedades Principales

### wasOpenGifts

- Tipo: bool
- Descripción: Indica si se han abierto regalos en el combate
- Acceso: get/set

### IsFirtTurn

- Tipo: bool
- Descripción: Indica si es el primer turno del combate
- Acceso: get/set
- Nota: Solo se puede establecer internamente

### element

- Tipo: Element
- Descripción: Elemento del combate actual
- Acceso: get/set

### rarity

- Tipo: CreatureType
- Descripción: Rareza de los enemigos en el combate
- Acceso: get/set

### hero

- Tipo: Hero
- Descripción: Referencia al héroe que participa en el combate
- Acceso: get
- Nota: Se obtiene automáticamente del equipo de aliados

### enemies

- Tipo: Team
- Descripción: Equipo de enemigos en el combate
- Acceso: get/set

### allies

- Tipo: Team
- Descripción: Equipo de aliados en el combate
- Acceso: get/set

### turn

- Tipo: Team
- Descripción: Equipo cuyo turno actual se esta jugando
- Acceso: get/set

<!-- ### combatMono

- Tipo: CombatMono
- Descripción: Referencia al componente visual del combate
- Acceso: get/set -->

# Métodos Principales

## Inicialización y Control

### StartCombat()

- Inicia o continúa un combate existente
- Prepara todos los sistemas necesarios
- Ejecuta efectos de inicio de combate

### EndCombat()

- Finaliza el combate actual
- Notifica a todas las unidades
- Maneja la limpieza de recursos

### ExecuteTurn()

- Ejecuta el turno actual
- Coordina las acciones de todas las unidades
- Verifica condiciones de finalización

## Gestión de Equipos

### AddEnemies(Unit[] enemies)

- Agrega nuevos enemigos al combate
- Actualiza el estado del equipo enemigo

### AddAllies(Creature[] allies)

- Agrega nuevos aliados al combate
- Actualiza el estado del equipo aliado

## Consultas

### GetEnemiesFrom(Unit unit)

- Retorna la lista de enemigos de una unidad específica
- Útil para cálculos de daño y efectos

### GetAlliesFrom(Unit unit, bool full = false)

- Retorna la lista de aliados de una unidad
- Opcionalmente incluye la unidad consultante

# Clase Team

La clase Team representa un equipo en el combate, manejando la colección de unidades y su comportamiento grupal.

## Características Principales

| Característica | Descripción |
| --- | --- |
| Gestión de Unidades | Maneja la colección de unidades del equipo |
| Sistema de Slots | Controla la posición de las unidades en el campo de batalla |
| Prioridad de Acción | Determina el orden de acción de las unidades |
| Estado del Equipo | Mantiene el estado general del equipo (victoria/derrota) |

## Propiedades Principales

### combatTeam

- Tipo: CombatTeam
- Descripción: Identifica si es equipo aliado o enemigo
- Acceso: get/set

### units

- Tipo: List<Unit>
- Descripción: Lista de unidades activas en el equipo
- Acceso: get

### fullUnits

- Tipo: List<Unit>
- Descripción: Lista completa de unidades del equipo
- Acceso: get/set

### hero

- Tipo: Hero
- Descripción: Referencia al héroe del equipo (si existe)
- Acceso: get

# Métodos Principales

## Gestión de Unidades

### Add(Unit unit)

- Agrega una nueva unidad al equipo
- Actualiza la posición en el campo de batalla
- Notifica al sistema visual

### Remove(Unit unit)

- Elimina una unidad del equipo
- Actualiza el estado del equipo

## Control de Turnos

### OnNextTurn(Team turn, bool fromContinue = false)

- Maneja el inicio del turno del equipo
- Coordina las acciones de todas las unidades

### HeroStartTurn()

- Inicia el turno específico del héroe
- Prepara las acciones del héroe

# Notas Importantes

- La clase utiliza un sistema de slots para manejar la posición de las unidades en el campo de batalla
- El sistema de turnos es asíncrono, permitiendo acciones complejas y continuas
- La persistencia de datos se maneja mediante serialización JSON
- El sistema está diseñado para manejar tanto combates regulares como combates personalizados (Modos de prueba)

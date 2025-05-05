# Sistema de partidas

La clase [`Game`](../../../Assets/src/app/Game/Game/Game.cs) es un ScriptableObject que actúa como el núcleo central del sistema de juego, manejando la inicialización, persistencia de datos y control general del estado del juego. Hereda de ScriptableObject y está diseñada para mantener el estado global del juego, incluyendo el héroe, torres, eventos y progresión.

# Características Principales

| Característica | Descripción |
| --- | --- |
| Estado Global | Maneja el estado completo del juego |
| Persistencia | Controla la serialización y deserialización de datos |
| Inicialización | Coordina la creación de instancias del juego |
| Gestión de Eventos | Administra eventos disponibles y agregados |
| Control de Torre | Maneja la progresión entre diferentes torres |

# Referencia de Propiedades

## Datos Persistentes

### combatPersistentData

- Tipo: CombatPersistentData
- Descripción: Almacena datos persistentes de combate
- Serialización: JSON con manejo de valores null

### currentScene

- Tipo: string
- Descripción: Escena actual del juego
- Acceso: get/set privado
- Valor inicial: "Floor"

### ascensionLevel

- Tipo: AscensionLevel
- Descripción: Nivel de ascensión actual
- Serialización: Campo serializado con JSON reference

## Elementos del Juego

### hero

- Tipo: `Hero`
- Descripción: Héroe actual en el juego
- Serialización: Required y referenciado
- Acceso: get/set privado

### towers

- Tipo: `List<Tower>`
- Descripción: Lista de torres disponibles
- Serialización: Required
- Acceso: get/set privado

### events

- Tipo: `List<Event>`
- Descripción: Lista de eventos en el juego
- Serialización: Referencias individuales
- Acceso: get/set privado

## Propiedades Calculadas

### hasData

- Tipo: bool
- Descripción: Indica si existe un héroe asignado
- Acceso: get público

### aviableEvents

- Tipo: `List<Event>`
- Descripción: Lista de eventos disponibles para agregar
- Acceso: get público
- Filtra eventos no agregados (isAdded = false)

# Métodos Principales

## Inicialización

### Initialize(Hero hero, AscensionLevel ascensionLevel, `List<Relic>` relics)

- Inicializa una nueva instancia del juego
- Configura el nivel de ascensión y datos de combate
- Instancia el héroe con reliquias
- Prepara las torres y eventos

### CreateInstance(Hero hero, AscensionLevel ascensionLevel, `List<Relic>` relics)

- Crea una nueva instancia del juego
- Invoca la inicialización
- Retorna la instancia creada

## Control de Juego

### NextFloor()

- Avanza al siguiente piso en la torre actual
- Delega la operación a la torre actual

### NextTower()

- Cambia a la siguiente torre en la secuencia
- Verifica límites y actualiza la torre actual

## Gestión de Estado

### EndGame()

- Finaliza el juego exitosamente
- Actualiza el controlador de niveles desbloqueados

### Lose()

- Marca el juego como perdido
- Establece IsEnd en true

## Persistencia

### OnSave()

- Guarda el estado actual del juego
- Registra la escena activa

# Notas

<!-- - La clase implementa un patrón Factory para crear instancias del juego -->
- Utiliza serialización JSON avanzada con referencias y manejo de null
- Implementa sistema de eventos asíncrono para carga de recursos
- Proporciona métodos de prueba para desarrollo y testing
- Maneja la persistencia de datos entre sesiones de juego

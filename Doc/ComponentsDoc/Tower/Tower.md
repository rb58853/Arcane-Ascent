# Sistema de Torre

La clase [`Tower`](../../../Assets/src/app/Tower/Tower.cs) es un ScriptableObject que representa una torre en el juego, manejando múltiples pisos (floors) y su progresión. Se puede crear mediante el menú de Unity como "Tower/Tower" y se inicializa con un nivel específico.

## Características Principales

| Característica | Descripción |
| --- | --- |
| Niveles | Gestiona múltiples pisos en una estructura jerárquica |
| Progresión | Controla el avance entre pisos y torres |
| Combate | Maneja la lógica de combate entre pisos |
| Persistencia | Implementa sistema de guardado automático al avanzar |

# Referencia de Propiedades

## Propiedades Principales

### floors

- Tipo: `List<Floor>`
- Descripción: Lista de todos los pisos en la torre
- Acceso: get/set privado
- Uso: Almacena la estructura completa de la torre

### level

- Tipo: `int`
- Descripción: Número de identificación de la torre
- Acceso: get/set privado
- Uso: Identifica la torre en el juego (Torre #1, #2, etc.)

### currentFloor

- Tipo: `Floor`
- Descripción: Piso actual donde se encuentra el jugador
- Acceso: get/set privado
- Uso: Referencia al piso en curso

### elitsRange

- Tipo: `List<ElitRange>`
- Descripción: Rangos donde aparecen enemigos elite
- Acceso: get/set privado
- Uso: Define la probabilidad de aparición de elites

## Propiedades de Estado

### doorsOver

- Tipo: `Dictionary<Element, int>`
- Descripción: Contador de puertas finalizadas por elemento
- Acceso: get privado
- Uso: Rastrea el progreso en puertas específicas

### maxFloorLevel

- Tipo: `int`
- Descripción: Número total de pisos en la torre
- Acceso: get
- Uso: Propiedad calculada a partir de floors.Count

# Métodos Principales

## Inicialización

### CreateInstance(Game game)

- Crea una nueva instancia de la torre
- Inicializa la torre con el juego proporcionado
- Retorna la instancia creada

### Initialize(Game game)

- Inicializa la torre con el juego
- Configura el primer piso como actual
- Inicializa todos los pisos de la torre

## Progresión

### NextFloor()

- Avanza al siguiente piso en la torre
- Inicia nuevo combate si es necesario
- Guarda el progreso automáticamente
- Retorna el nombre de la escena a cargar

### ForceCurrentFloor(Floor floor)

- **USO DESARROLLO**: Fuerza el cambio al piso especificado
- No debe usarse en producción

## Gestión de Estado

### AddOverDoor(Door door)

- Registra una puerta finalizada
- Actualiza el contador para el elemento específico

### UpdateAfterLoad()

- Actualiza el estado de todos los pisos después de cargar
- Mantiene la consistencia del estado de la torre

# Clase ElitRange

### Propiedades

#### probability

- Tipo: int
- Valor por defecto: 20
- Descripción: Probabilidad base de aparición de elite

#### min/max

- Tipo: int
- Descripción: Rango de pisos donde puede aparecer el elite

#### Over

- Tipo: bool
- Descripción: Indica si el rango está completado

### Métodos

#### Probability(int floorLevel)

- Calcula la probabilidad real de aparición del elite
- Considera el rango y si está completado
- Retorna 100% en el último piso del rango

# Notas

- La clase implementa un sistema de guardado automático al avanzar entre pisos
- El sistema de elites es configurable por rangos de pisos
- La torre mantiene un registro de puertas finalizadas por elemento
- La inicialización es asíncrona para mejor rendimiento

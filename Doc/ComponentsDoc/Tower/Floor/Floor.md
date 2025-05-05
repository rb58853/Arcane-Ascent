# Sistema de Pisos

La clase [`Floor`](../../../../Assets/src/app/Tower/Floor/Floor.cs) es una clase serializable que maneja el funcionamiento de los pisos en el juego. Esta clase es instanciable en todo momento y se encarga de gestionar elementos como puertas, eventos, combates y desbloqueo de cartas.

## Características Principales

| Característica | Descripción |
| --- | --- |
| Nivel | Controla el nivel del piso en la torre |
| Tipo | Define el tipo de piso (común, elit, jefe, etc.) |
| Puertas | Gestiona las puertas del piso |
| Eventos | Maneja eventos especiales(desarrollo) y aleatorios |
| Cartas | Controla el desbloqueo de nuevas cartas |

## Tipos de Pisos

La clase FloorType define los diferentes tipos de pisos disponibles:

```csharp
public enum FloorType
{
    commun,    // Piso común
    elit,      // Piso elit
    towerboss, // Piso del jefe de torre
    finalboss, // Piso del jefe final
    store,     // Piso tienda
    restoreZone // Zona de restauración
}
```

## Propiedades Principales

### level

- Tipo: int
- Acceso: get/set
- Descripción: Nivel del piso en la torre
- Uso: Controla la progresión y dificultad

### tower

- Tipo: Tower
- Acceso: get/set
- Descripción: Referencia a la torre que contiene el piso
- Uso: Proporciona acceso al contexto de la torre

### powerLevel

- Tipo: int
- Acceso: get/set
- Descripción: Nivel de poder del piso
- Uso: Afecta la dificultad de los enemigos y recompensas

### floorType

- Tipo: FloorType
- Acceso: get/set
- Descripción: Tipo de piso actual
- Uso: Determina el comportamiento y configuración del piso

### eventSpawnProbability

- Tipo: int
- Valor por defecto: 20
- Descripción: Probabilidad de que aparezca un evento
- Uso: Controla la frecuencia de eventos especiales

## Sistema de Puertas

### GenerateDoors()

- Genera las puertas del piso según su tipo
- Implementa lógica diferente para cada tipo de piso
- Maneja puertas especiales (tienda, restauración)

### DoorsFinalBoss()

- Genera una única puerta para el jefe final
- Elemento neutral para el combate final

### DoorsTowerBoss()

- Genera una puerta para el jefe de torre
- Elemento aleatorio de la configuración

### DoorsCommunElit()

- Genera 3 puertas para pisos comunes y elit
- Incluye puertas especiales y elit
- Distribuye las puertas estratégicamente

## Sistema de Eventos

### GenerateEvent()

- Genera eventos aleatorios basados en la probabilidad
- Selecciona eventos de la lista de eventos disponibles
- Implementa sistema de instancia de eventos

## Sistema de Cartas

### UnlockCardsOnEnter()

- Gestiona el desbloqueo de cartas al entrar al piso
- Verifica la compatibilidad con el héroe actual
- Limita el número máximo de cartas desbloqueables

## Métodos Principales

### Initialize(Tower tower)

- Inicializa el piso con su nivel y torre
- Genera las puertas iniciales
- Prepara el piso para su uso

### SetCombat(Combat combat)

- Asigna un combate a una puerta
- Maneja el estado de combate del piso

### SetEvent(Event event_)

- Establece un evento en el piso
- Crea una instancia del evento si es necesario

### EndFloor()

- Finaliza todos los combates activos
- Prepara el piso para su cierre

### UpdateAfterLoad()

- Actualiza el estado de los conjuntos de cartas
- Se ejecuta después de cargar el piso

# Notas

- La clase utiliza serialización avanzada con [Serializable] y [JsonObject(IsReference = true)]
- Implementa un sistema flexible de generación de contenido
- Maneja diferentes tipos de pisos con comportamientos únicos
- Integra sistemas de eventos y desblqueo de cartas en la progresión del juego

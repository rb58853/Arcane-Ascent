# Sistema de Eventos

La clase [`Event`](../../../Assets/src/app/Event/Event.cs) es un ScriptableObject que representa un evento en el juego, manejando interacciones, acciones y resultados. Esta clase es la base para crear eventos personalizados que pueden ser instanciados y configurados en tiempo de ejecución.

# Características Principales

| Característica | Descripción |
| --- | --- |
| Referencia de Activos | Maneja referencias a activos mediante assetReference |
| Sistema de Instancia | Permite crear instancias únicas de eventos base |
| Gestión de Acciones | Maneja múltiples acciones con resultados probabilísticos |
| Localización | Soporta texto multilingüe mediante LanguageTextSet |
| Posicionamiento | Controla la ubicación física del evento en la escena |

# Referencia de Propiedades

## Propiedades Principales

### description

- Tipo: string
- Descripción: Descripción base del evento
- Acceso: get/set
- Uso: Texto descriptivo que se muestra al jugador

### position

- Tipo: EventPosition
- Descripción: Ubicación física del evento en el espacio
- Valores posibles: Floor, Roof, Wall, Door
- Uso: Determina dónde se puede colocar el evento

### prefab

- Tipo: EventMono
- Descripción: Prefab visual del evento
- Acceso: get/set
- Uso: Define la representación visual del evento

### eventActions

- Tipo: List<EventAction>
- Descripción: Lista de acciones disponibles en el evento
- Acceso: get/set
- Uso: Contiene las opciones interactivas del evento

## Propiedades de Estado

### isOver

- Tipo: bool
- Descripción: Indica si el evento ha terminado
- Acceso: get/set
- Uso: Controla el estado de finalización del evento

### isAutomatic

- Tipo: bool
- Descripción: Determina si el evento se activa automáticamente
- Acceso: get/set
- Uso: Controla la activación del evento

### isInstance

- Tipo: bool
- Descripción: Indica si es una instancia del evento base
- Acceso: get/set
- Uso: Distingue entre eventos base e instancias

# Métodos Principales

## Métodos de Instancia

### CreateInstance()

- Crea una nueva instancia del evento
- Configura el estado como instancia
- Retorna la nueva instancia

### CreateInstanceByBaseAddressable(bool isOver)

- Crea una instancia desde el evento base addressable
- Permite especificar el estado de finalización
- Útil para la carga de eventos

## Métodos de Control

### EndEvent()

- Marca el evento como terminado
- Desactiva la visualización del prefab

### SetPosition(GameObject slot)

- Establece la posición física del evento
- Ajusta la escala y posición del prefab
- Retorna la posición final establecida

# Clase EventAction

## Propiedades

### action

- Tipo: string
- Descripción: Acción que se puede realizar
- Acceso: get/set
- Uso: Define la acción disponible al jugador

### actionText

- Tipo: string
- Descripción: Texto localizado de la acción
- Acceso: get
- Uso: Muestra el texto en el idioma actual

### image

- Tipo: Sprite
- Descripción: Icono visual de la acción
- Acceso: get/set
- Uso: Representación visual de la acción

## Métodos

### CanUseItem(EventItem item)

- Verifica si un item puede ser usado en la acción
- Compara el item con el itemBase requerido

### Result()

- Calcula el resultado de la acción
- Considera probabilidades y suerte del héroe
- Retorna el resultado seleccionado

# Clase EventResult

## Propiedades

### resultType

- Tipo: ResultType
- Descripción: Tipo de resultado (None, Good, Bad)
- Acceso: get/set
- Uso: Determina el impacto del resultado

### description

- Tipo: string
- Descripción: Descripción del resultado
- Acceso: get/set
- Uso: Texto que describe el resultado

### probability

- Tipo: int
- Descripción: Probabilidad de ocurrencia
- Acceso: get
- Uso: Calcula la probabilidad final considerando la suerte

# Notas

<!-- * La clase utiliza el sistema de Addressables para la gestión de activos -->
<!-- * Implementa un sistema de localización para texto multilingüe -->
- Los prefabs se instancian de manera lazy para optimizar recursos
- Los resultados consideran la suerte del héroe en sus probabilidades
- El sistema de eventos es extensible mediante herencia

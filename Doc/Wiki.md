# Acane Ascent Wiki

Arcane Ascent es un videojuego desarrollado en el motor grafico **Unity** con lengueje de programacion principal **C#**. Este documento recoge la documentacion necesaria para extender el proyecto en si, tanto a nivel de codigo como a nivel de editor. El codigo esta debidamente documentado para lograr un flujo de trabajo comodo mientras se esta desarrollando. Por otro lado la estructura, detalles del editor, y especificidades del codigo del proyecto seran presentadas y explicadas en este documento.

# Entidades

En Arcane las entidades ([`Entitie`](../Assets/src/app/Entitie/Entity.cs)) estan dadas por 5 entidades basicas: cartas, unidades, efectos, habilidades e items. Cada una de ellas cumple su funcion individual y se fusionan para conformar el correcto funcionamiento del proyecto.

### Propiedades

Cada una de las entidades del juego tienen propiedades básicas que se mencionan a continuación.

- `ID`: string que representa el id de la unidad, este id es muy importante a nivel lógico, por ejemplo, los efectos lo usan para reconocer que son del mismo tipo, pasa igual con las unidades invocadas. Si una entidad no posee un `ID` que se le haya dado manualmente, entonces el nombre del `ScriptableObject` que representa la entidad, pasará a usarse como ID de la misma.
- `name`: Nombre de la entidad, esto se usa como una base para guía, dado que el nombre real mostrado en el juego está dado por el sistema de lenguajes.
- `description`: Descripción de la entidad, esto se usa como una base para guía, dado que la descripción real mostrada en el juego está dada por el sistema de lenguajes.
- `LanguageID`: Indica que la entidad usa un lenguaje, que tendrá nombre y descripción desde el sistema de lenguajes, en caso de estar vacío este campo entonces no tendrá texto dentro del juego. Las entidades con igual `LanguageID` comparten los mismos textos. Es un error usar el mismo `LanguageID` para entidades que no tengan exactamente el mismo nombre y descripción, la automatización de llenado de lenguajes usará uno solo de los (nombre - descripción).
- [`LanguageSet`](../Assets/src/app/Entitie/LanguageTextSet.cs): [Diccionario](../Assets//src/config/Languages/Data/EntitiesLanguageJson.json) con el conjunto de lenguajes con descripción y texto por cada uno de los [`LangISO`](../Assets//src/config/Languages/AviableLanguages.cs) disponibles. Este diccionario o lenguajes puede ser rellenado manualmente o automatizarlo desde [`Editor > Window > Tools > Languages`](../Assets/src/app/Utils/MenuTools/LanguagesSetJSON.cs).

### Uso de Instancias

Es muy importante usar siempre instancias de cada una de las entidades y no las entidades bases en sí. Usar las entidades bases hace cambios en la raíz de los scriptableObject y los modifican de forma permanente. Cada una de las entidades del proyecto implementan el método `CreateInstance()`. Para usar este método se usa cualquier entidad (por lo general es la base), a esta entidad se le hace algo como:

```csharp
Card myCard = aBaseCard.CreateInstance();
```

Esto internamente se encarga de crear una instancia idéntica a la carta `baseCard` y la devuelve en este método `CreateInstance()`.

```csharp
public virtual Card CreateInstance(Card inherit)
{
    base.Instantiate();
    Card instance = ScriptableObject.Instantiate(this);
    instance.Initialize(inherit.hero);
    instance.inheritCard = inherit;
    return instance;
}
```

## Cartas

Las cartas representan el mecanismo fundamental mediante el cual los Heroes ejecutan sus habilidades y ataques en el juego. Este componente actúa como el sistema central de coordinación, gestionando y ejecutando la mayoría de las acciones que los jugadores realizan durante el juego.

Para obtener información detallada sobre la implementación y funcionalidades específicas de las cartas, puede consultar la documentación completa en [Ver más](./ComponentsDoc/Cards/Cards.md).

## Unidades

Las unidades representan las entidades que interactuan en el combate, lo que viene siendo los heroes, los enemigos y las invocaciones.

Para obtener información detallada sobre la implementación y funcionalidades específicas de las cartas, puede consultar la documentación completa en [Ver más](./ComponentsDoc/Units/Units.md).

## Efectos

Los efectos representan estados incrustados sobre unidades, los buffs y debuffs sobre las mismas. Existen muchas opciones de effectos, desde danno y regeneracion hasta estados de desarmado o aturdido.

Para obtener información detallada sobre la implementación y funcionalidades específicas de las cartas, puede consultar la documentación completa en [Ver más](./ComponentsDoc/Effects/Effects.md).

## Habilidades

Las hablidades representan los hechizos o todo lo que se pueda lanzar que cause un efecto. Esta clase proporciona la funcionalidad base para manejar habilidades que pueden ser ejecutadas por unidades o cartas, incluyendo características como tipos de lanzamiento, objetivos automáticos y efectos visuales.

Para obtener información detallada sobre la implementación y funcionalidades específicas de las cartas, puede consultar la documentación completa en [Ver más](./ComponentsDoc/Skills/Skills.md).

## Eventos

La clase `Event` es un ScriptableObject que representa un evento en el juego, manejando interacciones, acciones y resultados. Esta clase es la base para crear eventos personalizados que pueden ser instanciados y configurados en tiempo de ejecución.

Para obtener información detallada sobre la implementación y funcionalidades específicas de las cartas, puede consultar la documentación completa en [Ver más](./ComponentsDoc/Events/Events.md).

# Conceptos o Componentes

## Damage

El componente `Damage` es una clase fundamental en el sistema de combate que gestiona todos los aspectos relacionados con el daño en el juego. Este componente centraliza la lógica para:

- Controlar el cálculo base del daño
- Gestionar las habilidades que se activan durante eventos de daño
- Administrar modificadores que afectan la cantidad de daño
- Mantener el estado y valor actual del impacto

Para obtener información detallada sobre la implementación y funcionalidades específicas este sistema, puede consultar la documentación completa en [ver mas](./ComponentsDoc/Attack/Damage.md).

## Attack

El componente `Attack` es un sistema serializable diseñado específicamente para la gestión y modificación de los parámetros de ataque. Este componente centraliza la funcionalidad de asignación y modificación de valores de ataque, permitiendo una gestión flexible y estructurada de los parámetros de combate.

Para obtener información detallada sobre la implementación y funcionalidades específicas del sistema de ataque, puede consultar la documentación completa en [ver más](./ComponentsDoc/Attack/Attack.md).

## Nivel de Ascension

La clase `AscensionLevel` representa un nivel de ascensión en el juego, manejando las modificaciones de dificultad y características especiales de cada nivel. Esta clase es serializable y utiliza referencias JSON para persistencia de datos.

Para obtener información detallada [ver más](./ComponentsDoc/Player/AscensionLevel.md).

# Escenas y estructuras imprescindibles

## Combate

La clase [`Combat`](../../../Assets/src/app/Combat/Combat.cs) representa un sistema de combate completo en el juego, manejando turnos, equipos y unidades en combate. Esta clase es la columna vertebral del sistema de combate, coordinando todas las interacciones entre unidades y equipos.

Para obtener información detallada [ver más](./ComponentsDoc/Combat/Combat.md).

## Torre

La clase [`Tower`](../../../Assets/src/app/Tower/Tower.cs) es un ScriptableObject que representa una torre en el juego, manejando múltiples pisos (floors) y su progresión. Se puede crear mediante el menú de Unity como "Tower/Tower" y se inicializa con un nivel específico.

Para obtener información detallada [ver más](./ComponentsDoc/Tower/Tower.md).

## Piso

La clase [`Floor`](../../../../Assets/src/app/Tower/Floor/Floor.cs) es una clase serializable que maneja el funcionamiento de los pisos en el juego. Esta clase es instanciable en todo momento y se encarga de gestionar elementos como puertas, eventos, combates y desbloqueo de cartas.
Para obtener información detallada [ver más](./ComponentsDoc/Tower/Floor/Floor.md).

# Puertas

La clase [`Door`](../../../../Assets/src/app/Tower/Door/Door.cs) es una clase serializable que maneja la lógica de las puertas en el juego. Cada puerta tiene un elemento asociado, un nivel de rareza y puede ser especial (tienda, zona de restauración, etc.). Esta clase es fundamental para el sistema de combate y progresión del juego.

Para obtener información detallada [ver más](./ComponentsDoc/Tower/Door/Door.md).

## Partida

La clase [`Game`](../../../Assets/src/app/Game/Game/Game.cs) es un ScriptableObject que actúa como el núcleo central del sistema de juego, manejando la inicialización, persistencia de datos y control general del estado del juego. Hereda de ScriptableObject y está diseñada para mantener el estado global del juego, incluyendo el héroe, torres, eventos y progresión.

Para obtener información detallada [ver más](./ComponentsDoc/Game/Game.md).

# Interfaces

## OnDamage

El `interface IOnDamage` es un componente fundamental que permite a las entidades del juego responder a eventos de daño. Este interface define el contrato que deben cumplir todos los efectos, habilidades, items y otras entidades que necesiten reaccionar cuando se produce un evento de daño.

Para implementar IOnDamage, cualquier entidad debe proporcionar una implementación del método `OnDamage(Unit target, Damage damage)`. Este método actúa como un punto de entrada unificado que se activa tanto cuando la entidad recibe daño como cuando la entidad causa daño a otros.

El método OnDamage recibe dos parámetros fundamentales:

- `Unit target`: La unidad que está involucrada en el evento de daño
- `Damage damage`: El objeto que contiene toda la información relevante sobre el daño producido

Para obtener información detallada sobre la implementación y funcionalidades específicas del `interface IOnDamage`, puede consultar la documentación completa en [ver mas](./ComponentsDoc/Interfaces/IOnDamage.md)

# Sistema de Guardado

Para el sistema de guardado se utiliza la biblioteca `Newtonsoft.Json`. Esta biblioteca guarda datos en forma de `JSON`, que funciona correctamente para `ScriptableObject` o para `MonoBehaviour`. Es importante tener en cuenta que para cargar en un `ScriptableObject` solo se puede usar la función `Newtonsoft.Json.FromJsonOverwrite(json, myObject)`. Para más información de esta biblioteca consulta la [documentación oficial de unity](https://docs.unity3d.com/Packages/com.unity.nuget.newtonsoft-json@3.0/manual/index.html).

Para más información del sistema de guardados consultar la documentación de todo el [Sistema de Guardado](./ComponentsDoc/SaveSystem/SavedSistem.md) que se sigue en el proyecto.

<!-- # Control de Prefabs
https://docs.unity3d.com/Manual/AssetBundles-Native.html -->

# Advertencia ⚠️

### 1. Todo las escenas deberian estar en un canvas con camara. Los `Drags` no van a funcionar en canvas sin camara. Los volumenes de renderizado no van a funcionar sin camara

### 2. Nunca se debe usar los `ScriptableObject` bases, siempre se deben usar instancias de estas. Cada una de las entidades tiene su forma de crear instancia, lo cual esta definido en el apartado de [Entidades](#uso-de-instancias)

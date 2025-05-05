# Wiki

Documentacion de codigo del proyecto Arcane.

# Entidades

En Arcane las entidades estan dadas por 5 entidades basicas: cartas, unidades, efectos, habilidades e items. Cada una de ellas cumple su funcion individual y se fusionan para conformar el correcto funcionamiento del proyecto.

### Propiedades

Cada una de las entidades del juego tienen propiedades basicas que se mencionan a continuacion.

- `ID`: string que representa el id de la unidad, este id s muy importante a nivel logico, por ejemplo, los efectos lo usan para reconocer que son del mismo tipo, pasa igual con las unidades invocadas. Si una entidad no posee un `ID` que se le haya dado manualmente, entonces el nombre del `ScriptableObject` que representa la entidad, pasara a usarse como ID de la misma.
- `name`: Nombre de la entidad, esto se usa como una base para guia, dado que el nombre real mostrado en el juego esta dado por el sistema de lenguajes.
- `description`: Descripcion de la entidad, esto se usa como una base para guia, dado que la descripcion real mostrada en el juego esta dada por el sistema de lenguajes.
- `LanguageID`: Indica que la entidad usa un lenguage, que tendra nombre y descripcion desde el sistema de lenguajes, en caso de estar vacio este campo entonces no tendra texto dentro del juego. Las entidades con igual `LanguageID` comparten los mismos textos. Es un error usar el mismo `LanguageID` para entidades que no tengan exactamente el mismo nombre y descripcion, la automatizacion de llenado de lenguages usara uno solo de los (nombre - descripcion).
- `LanguageSet`: Diccionario con el conjunto de lenguages con descripcion y texto por cada uno de los [`LangISO`](TODO) disponibles. Este diccionario o lenguages puede ser rellenado manualmente o automatizarlo desde [`Editor > Window > Tools > Languages`](TODO).

## Cartas

Las cartas, como concepto, son la forma de lanzar habilidades o ataques de los `Heroes`.

### Propiedades
<!-- TODO: Imagen para cada una de las propiedades -->
Las propiedades basicas de todas las cartas son de las cartas son:

- `CardType`: Cada una de las cartas tiene su tipo. Esta propiedad es ajustable de forma manual y solo afecta a la informacion de la carta.
  - **Skill**: Hace referencia a las cartas que usan [`Skill`]().
  - **Attack** : Hace referencia a las cartas que usan [`Attack`]().
  - **Invocacion**:Hace referencia a las cartas que usan [`SummonSummon`](). La invocacion es una subclase de `Skill`.
- `DiscardType`: La forma de descrte de las cartas que se divide en
  - **Ethereal**: Se van al [cementerio]() al salir de la mano.
  - **Drip**: Se van al [cementerio]() al ser jugadas.
  - **Discard**: Al ser jugadas se van a la [pila de descartes]().
- `TeamTarget`: Equipo al cual se le puede lanzar la carta en caso de ser target.
  - **Allies**: Solo pueden seleccionarse aliados como objetivo.
  - **Enemies**: Solo pueden seleccionarse enemigos como objetivo.
  - **Both**: Pueden seleccionarse aliados o enemigos como objetivo..
- `ManaCost`: Numero entero que representa el costo base de energia por usar la carta.

### Skill Card

Las cartas de habilidades son una subclase de las cartas (`SkillCard:Card`) que utilizan una `Skill` pasada por parametros a la hora de ser creada, esta habilidad es lanzada de forma automatica o en forma de target, segun se especifique en el tipo de lanzamiento de la carta.
Las cartas de invocacion son tambien `SkillCard` que se le pasa una habilidad de tipo `SummonSummon`.

#### Propiedades

- `skill`: Habilidad en cuestion que se le pasa a la carta para ser lanzada en el momento que se use la carta.

### AttackCard

Las cartas de habilidades son una subclase de las cartas (`AttackCard:Card`) que utilizan un `Attack` pasado por parametros a la hora de ser creada, este ataque es lanzado en el momento que se activa la carta. El tipo de ataque, enemigos que golpea y demas propiedades del ataque corren por parte del campo especial `Attack`.

#### Propiedades

- `attack`: Ataque en cuestion que se llena en el inspector desde la propia carta.

## Unidades

Las unidades son la

## Efectos

## Habilidades

## Eventos

# Conceptos

## Damage

## Attack

# Campos

## Priority

El concepto `prioridad` se usa para establecer el orden de ejecuciones.

### OnDamage Priority

Esta prioridad se usa para establecer el orden en que se activan las acciones [`IOnDamage`](Doc/Actions/IOnDamage.md). Por ejemplo, cuando una entidad es `Intagible`, esta reduce el danno a cero, pero nosotros queremos devolver el danno con un efecto `Reflexion`, es entonces prioritario entrar primero a reflexion antes de reducir el danno a cero.

```c#
//En este ejemplo podriamos tener algo como
Reflexion.onDamagePriority => 0;
Intangible.onDamagePriority => 1;
```

De esta forma primero se ejecutara `Reflexion` y luego `Intangible` dado que `Reflexion.onDamagePriority < Intangible.onDamagePriority`.

# Sistema de Guardado

Para el sistema de guardado se utiliza la biblioteca `Newtonsoft.Json`. Esta biblioteca guarda datos en forma de `JSON`, que funciona correctamente para `ScriptableObject` o para `MonoBehaviour`. Es importante tener en cuenta que para cargar en un ScriptableObject solo se puede usar la funcion `Newtonsoft.Json.FromJsonOverwrite(json, myObject)`. Para mas informacion de esta biblioteca consulta la [documentacion oficial de unity](https://docs.unity3d.com/Packages/com.unity.nuget.newtonsoft-json@3.0/manual/index.html).

Para un mas informacion de sistema de guardados consultar la documentacion de todo el [Sistema de Guardado](Doc/SaveLoad/SavedSistem.md) que se sigue en el proyecto.

<!-- # Control de Prefabs
https://docs.unity3d.com/Manual/AssetBundles-Native.html -->

# Importante

- Todo las escenas deberian estar en un canvas con camara. Los `Drags` no van a funcionar en canvas sin camara. Los volumenes de renderizado no van a funcionar sin camara.
- Nunca se debe usar los `ScriptableObject` bases, siempre se deben usar instancias de estas. Cada una de las entidades tiene su forma de crear instancia, lo cual esta definido en el apartado de [Entidades](#entidades)

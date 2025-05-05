# Sistema de Cartas

Las cartas ([`class Card`](../../../Assets/src/app/Cards/Card.cs)), como concepto, son la forma de lanzar habilidades o ataques de los `Heroes`. Son un componente funcamental del juego que coordina y ejecuta la mayoria de las acciones del jugador.

## Caracteristicas

El sistema utiliza varios enums para controlar diferentes aspectos de las cartas:

### 1. Playable

| Estado | Descripción |
| --- | --- |
| True | Carta jugable |
| NoMana | Sin suficiente mana |
| OtherCard | Otra carta seleccionada |
| Stun | Aturdida (no pude usar cartas) |
| Disarm | Desarmada (no puede usar ataques) |
| Dishability | Deshabilitada (no puede usar habilidades) |
| NoSpace | Sin espacio para invocaciones |
| Unplayable | No jugable |

### 2. TargetTeam

| Tipo | Descripción |
| --- | --- |
| allies | Aliados |
| enemigos | Enemigos |
| both | Ambos |
| summonedAllies | Aliados invocados |

### 3. DiscardType

| Tipo | Descripción |
| --- | --- |
| Normal | Descarte normal (va a la pila de descartes) |
| Ethereal | Descarte etéreo (Va al cementerio siempre)|
| Drip | Descarte agota (Al jugars va al cementerio) |

### 4. CardType

| Tipo | Descripción |
| --- | --- |
| skill | Habilidad |
| attack | Ataque |
| summon | Invocación |
| any | Cualquiera |

## Estructura de Datos

La clase
 implementa un sistema flexible para manejar diferentes tipos de cartas:

### Propiedades Principales

| Propiedad | Tipo | Descripción | Uso |
| --- | --- | --- | --- |
| cardType | CardType | Tipo de carta | Define comportamiento base |
| element | Element | Elemento de la carta | Interacción con resistencias y habilidades |
| hero | Hero | Dueño de la carta | Referencia al jugador |
| costMana | int | Costo de mana | Requisito para jugar |
| discardType | DiscardType | Tipo de descarte | Determina destino final al salir de la mano |
| targetTeam | TargetTeam | Equipo objetivo | Control de selección de objetivo |

### Sistema de Serialización

El sistema utiliza varios atributos para controlar la serialización:

| Atributo | Uso |
| --- | --- |
| [Serializable] | Habilita serialización |
| [JsonRequired] | Campo obligatorio en JSON |
| [JsonIgnore] | Excluye del JSON |
| [JsonProperty] | Configura serialización JSON |

## Métodos Principales

### 1. Inicialización

```csharp
public virtual Card Initialize(Hero hero)
{
    this.baseDiscardType = this.discardType;
    this.baseCostMana = CostMana;
    this.hero = hero;
    this.OnChangeText();
    return this;
}
```

### 2. Ejecución

```csharp
public virtual void Execute()
{
    if (OnExecute != null)
        this.OnExecute.Invoke();
    if (this.useAsUpgrade && this.tempUpgradeCard.OnExecute != null)
        this.OnExecute.Invoke();
    if (this.hero.onPlayCard != null)
        this.hero.onPlayCard.Invoke();
    discardAction[this.useAsUpgrade ? this.tempUpgradeCard.discardType : discardType]
        .Invoke();
    this.hero.UseMana(this.useAsUpgrade ? this.tempUpgradeCard.costMana : this.costMana);
}
```

### 3. Verificación de Objetivos

```csharp
public virtual bool IsTargeable(Units.Unit unit)
{
    switch (targetTeam)
    {
        case TargetTeam.both:
            return true;
        case TargetTeam.enemies:
            return hero.enemies.Contains(unit);
        case TargetTeam.allies:
            Team allies = hero.team;
            return allies.Contains(unit);
        case TargetTeam.summonedAllies:
            return hero.SummonedAllies().Contains(unit);
    }
    return false;
}
```

## Sistema de Mejoras

El sistema implementa un mecanismo de mejoras temporales:

```csharp
public virtual void UpgradeOnCombat()
{
    this.useAsUpgrade = true;
    this.OnChangeText();
    if (this.prefab_ != null && this.prefab_.isActiveAndEnabled)
        this.prefab.UpdateUpgradeState();
}
```

## Gestión de Descarte

El sistema maneja diferentes tipos de descarte:

```csharp
protected virtual void ToGraveyard()
{
    this.hero.deck.ToGraveyard(this);
}

public virtual void ToDiscardPile()
{
    this.hero.deck.ToDiscardPile(this);
}
```

# Cartas de Habilidad

Las cartas de habilidades son una subclase de las cartas ([`SkillCard:Card`](../../../Assets/src/app/Cards/SkillCard/SkillCard.cs)) que utilizan una `Skill` pasada por parametros a la hora de ser creada, esta habilidad es lanzada de forma automatica o en forma de target, segun se especifique en el tipo de lanzamiento de la carta.
Las cartas de invocacion son tambien `SkillCard` que se le pasa una habilidad de tipo `SummonSummon`.

## Estructura Base

La clase
 extiende la funcionalidad base de
 para implementar cartas que contienen habilidades. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| skill | Habilidad asociada | Define el comportamiento principal |
| skillTrowType | Tipo de lanzamiento | Controla la mecánica de uso |
| maxTargets | Máximo de objetivos | Limita el número de targets |
| inheritTargetFromSkill | Herencia de objetivo | Controla el comportamiento de targeting |

## Propiedades Principales

| Propiedad | Tipo | Descripción | Uso |
| --- | --- | --- | --- |
| skill | Skill | Habilidad contenida | Define el efecto principal |
| skillTrowType | SkillTrowType | Tipo de lanzamiento | Determina la mecánica de uso |
| maxTargets | int | Número máximo de objetivos | Limita el targeting |
| inheritTargetFromSkill | bool | Herencia de objetivo | Controla el comportamiento de targeting |

## Métodos Principales

### 1. Inicialización

```csharp
public override Card Initialize(Hero hero)
{
    base.Initialize(hero);
    skill = skill.CreateInstance(this);
    skill.UseInherit = inheritTargetFromSkill;
    return this;
}
```

### 2. Verificación de Jugabilidad

```csharp
public override Playable canPlay =>
    !this.hero.canUseSkill ? Playable.Dishability
    : skill.canUse != Playable.True ? skill.canUse
    : base.canPlay;
```

### 3. Gestión de Texto

```csharp
public override string ToString()
{
    toString = this.description + (this.skill != null ? skill.Description() : "");
    return base.ToString();
}
```

# Cartas de Ataque

Las cartas de ataque son una subclase de las cartas ([`AttackCard:Card`](../../../Assets/src/app/Cards/AttackCard/AttackCard.cs)) que utilizan un `Attack` pasado por parametros a la hora de ser creada, este ataque es lanzado en el momento que se activa la carta. El tipo de ataque, enemigos que golpea y demas propiedades del ataque corren por parte del campo especial `Attack`.

## Estructura Base y Propiedades Principales

La clase
 extiende la funcionalidad base de
 para implementar cartas que contienen ataques. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| attack | Ataque asociado | Define el comportamiento de combate |

## Métodos Principales

### 1. Inicialización

```csharp
public override Card Initialize(Hero hero)
{
    base.Initialize(hero);
    attack.Initialize(this);
    return this;
}
```

### 2. Gestión de Daño

```csharp
public Queue<Damage> GetAttacks()
{
    return attack.Attacks();
}
```

### 3. Verificación de Jugabilidad

```csharp
public override Playable canPlay =>
    !this.hero.canAttack ? Playable.Disarm : base.canPlay;
```

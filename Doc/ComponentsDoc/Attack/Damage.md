# Damage

El componente Damage es una clase fundamental en el sistema de combate que gestiona todos los aspectos relacionados con el daño en el juego. Este componente centraliza la lógica para:

- Controlar el cálculo base del daño
- Gestionar las habilidades que se activan durante eventos de daño
- Administrar modificadores que afectan la cantidad de daño
- Mantener el estado y valor actual del impacto

El componente se encuentra implementado en [Damage.cs](../../../Assets/src/app/Attack/Damage.cs) y actúa como punto central para toda la lógica relacionada con el sistema de daños del juego.

## Estructura de Datos y Tipos

| Componente | Tipo | Descripción | Uso |
| --- | --- | --- | --- |
| DamageType | Enum | Define los tipos de daño posibles | Control de efectos de cadena |
| LinkChain | List<Unit> | Rastrea unidades afectadas | Prevención de efectos infinitos |
| criticMultiplier | float | Modificador de daño crítico | Ajuste de daño dinámico |
| OnApplyDamageActions | List<IOnApplyDamage> | Efectos de daño | Procesamiento de habilidades |
| onApplyDamageSills | List<Skill> | Habilidades activadas | Efectos adicionales |

# Constructores y Configuración Inicial

## Constructor Principal

```csharp
public Damage(
    int damage,
    Attack attack,
    DamageType type = DamageType.attack,
    Element element = Element.neutral,
    float criticMultiplier = 1
)
```

## Constructor Alternativo

```csharp
public Damage(
    int damage,
    Unit owner,
    DamageType type = DamageType.effect,
    Element element = Element.neutral,
    float criticMultiplier = 1
)
```

# Sistema de Prioridades y Efectos

| Componente | Función | Descripción |
| --- | --- | --- |
| OnApplyDamageInvoke | Gestión de efectos  | Procesa efectos pre-daño |
| VariateDamageByElement | Modificación de daño  | Ajusta daño por elementos |
| ActiveSkillsOnDamage | Activación de habilidades  | Ejecuta efectos post-daño |
| JustOnTakeDamageInvoke | Procesamiento básico  | Maneja daño directo |

# Operaciones Especiales

## Sistema de Reflexión

```csharp
public void Reflect()
{
    this.isReflected = true;
}
```

## Clonación y Duplicación

```csharp
public Damage Clone(bool useEffects = true)
```

## Operadores Sobrecargados

```csharp
public static Damage operator -(Damage damage, int value)
public static Damage operator +(Damage damage, int value)
```

# Consideraciones de Diseño

## Uso Temporal

- El objeto debe crearse y destruirse en cada uso
- No debe almacenarse como atributo permanente
- El campo damage es mutable por diseño

## Extensibilidad

- Estructura preparada para elementos
- Sistema de prioridades configurable
- Soporte para habilidades y efectos

## Rendimiento

- Optimizado para cálculos frecuentes
- Gestión eficiente de efectos
- Priorización de operaciones

# Casos de Uso Comunes

## Ataques Básicos

```csharp
var damage = new Damage(
    damage: 100,
    attack: my_creature.attack,
    type: DamageType.attack
);
```

## Efecto de quemadura

```csharp
var burnDamage = new Damage(
    damageAmount: 50,
    owner: casterUnit,
    type: DamageType.effect,
    element: Element.fire
);
```

## Daño Reflejado

```csharp
var reflectedDamage = damage.Clone();
reflectedDamage.ForceSetType(DamageType.reflect);
reflectedDamage.Reflect();
```

----
Este sistema proporciona una base sólida para manejar mecánicas de combate complejas mientras mantiene el código organizado y mantenible. Su diseño modular facilita la adición de nuevas características sin afectar la funcionalidad existente.

# Sistema de Ataques

## Estructura Base

La clase [`Attack`](../../../Assets/src/app/Attack/Attack.cs) es la base del sistema de ataques, permitiendo que las unidades o cartas tengan múltiples ataques en una lista. Cada ataque puede tener sus propias propiedades y comportamientos.

## Tipos de Ataques

El sistema implementa diferentes tipos de ataques a través de herencia:

| Tipo | Descripción | Características |
| --- | --- | --- |
| Attack | Base | Gestión de daño y habilidades |
| AttackByCard | Para cartas | Integración con sistema de cartas |
| AttackCreature | Para criaturas | Integración con sistema de criaturas |
| AttackByEffect | Para efectos | Daño tipo efecto con efecto como portador |
| AttackByItem | Para items | Integración con sistema de items |

## Propiedades Principales

| Propiedad | Tipo | Descripción | Uso |
| --- | --- | --- | --- |
| damageType | DamageType | Tipo de daño | Control de efectos de cadena |
| launchTimes | int | Veces que se lanza | Control de ataques múltiples |
| throwType | ThrowType | Tipo de lanzamiento | Determina mecánica de ataque (range, melee) |
| attackType | AttackType | Tipo de ataque | Define comportamiento (Target, Auto, Random) |
| attackRate | float | Tasa de ataque | Control de velocidad |
| stacks | int | Número de stacks | Multiplicador de golpes |
| element | Element | Elemento del ataque | Interacción con resistencias escalable |

## Sistema de Habilidades

El sistema implementa tres tipos de habilidades:

| Tipo | Descripción | Momento de Ejecución |
| --- | --- | --- |
| onApplyDamageByStack | Ejecutadas con cada daño o golpe| Durante el ataque |
| onThrowAttack | Ejecutadas al iniciar el ataque | Al comenzar el ataque |
| attackModification | Modifican el ataque | Al seleccionar el ataque desde el portador |

## Serialización

El sistema utiliza atributos de serialización para persistencia:

| Atributo | Uso |
| --- | --- |
| [Serializable] | Habilita serialización |
| [JsonRequired] | Campo obligatorio en JSON |
| [JsonIgnore] | Excluye del JSON |
| [JsonProperty] | Configura serialización JSON |

## Métodos Principales

### 1. Inicialización

```csharp
public void RefreshAttackState()
{
    foreach (Skill skill in attackModification)
        skill.Execute();
}
```

### 2. Generación de Daño o de Golpes para este aatque

```csharp
public virtual Queue<Damage> Attacks()
{
    return Attacks(this.damage, this.stacks);
}
```

### 3. Modificación de Propiedades

```csharp
public virtual void ForceSetDamage(int damage)
{
    this.damage = damage;
}
```

### 4. Gestión de Stacks

```csharp
public virtual void IncreseStacks(int stacks)
{
    this.stacks += stacks;
}
```

## Consideraciones de Diseño

1. **Herencia**
   - Cada tipo de ataque extiende la clase base
   - Permite especialización sin afectar la funcionalidad base

2. **Serialización**
   - Soporte completo para JSON
   - Referencias circulares manejadas

3. **Extensibilidad**
   - Sistema abierto para nuevos tipos de ataques
   - Facilidad para agregar nuevas propiedades

4. **Rendimiento**
   - Optimizado para cálculos frecuentes
   - Gestión eficiente de habilidades y efectos

Este sistema proporciona una base sólida para implementar mecánicas de combate complejas mientras mantiene el código organizado y mantenible. Su diseño modular facilita la adición de nuevas características sin afectar la funcionalidad existente.

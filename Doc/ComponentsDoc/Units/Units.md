# Sistema de Unidades

## Estructura Base

La clase [`Unit`](../../../Assets/src/app/Unit/Units.cs) es la base del sistema de unidades, implementando funcionalidades comunes para todos los tipos de unidades. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| health | Vida actual | Control de estado de vida |
| maxHealth | Vida máxima | Límite de vida |
| shield | Escudo | Protección adicional |
| ethereal | Estado etéreo | Control de atacabilidad |
| team | Equipo | Control de alianzas |

## Propiedades Principales

| Propiedad | Tipo | Descripción | Uso |
| --- | --- | --- | --- |
| health | int | Vida actual | Control de estado de vida |
| maxHealth | int | Vida máxima | Límite de vida |
| shield | int | Escudo | Protección adicional |
| ethereal | bool | Estado etéreo | Control de atacabilidad |
| team | Team | Equipo | Control de alianzas |
| element | Element | Elemento | Interacción con efectos |

## Sistema de Efectos

El sistema implementa dos tipos de efectos:

| Tipo | Descripción | Uso |
| --- | --- | --- |
| buffs | Efectos positivos | Mejoras temporales |
| debuffs | Efectos negativos | Penalizaciones temporales |

## Métodos Principales

### 1. Gestión de Vida

```csharp
public virtual bool VariateHealt(int value)
{
    if (this.isDeath)
        return false;
    if (value > 0)
        OnHealth(value);
    health += value;
    health = math.min(health, maxHealth);
    health = math.max(health, 0);
    if ((this.health / (float)this.maxHealth) <= percentageLifeToDeath / 100f)
        OnDeath();
    return true;
}
```

### 2. Sistema de Escudo

```csharp
public void BlockDamageByShield(Damage damage)
{
    if (this.shield > 0)
    {
        int blocked = math.min(damage, this.shield);
        damage -= blocked;
        this.VariateShield(-blocked);
        if (this.shield <= 0)
            foreach (Skill skill in Skills.onBreakShield)
                this.ThrowSkill(skill);
    }
    foreach (IOnTakeDamageAfterShield onTakeDamageAfterShield in new List<IOnTakeDamageAfterShield>(this.onTakeDamageAfterShieldActions))
        onTakeDamageAfterShield.OnTakeDamage(this, damage);
}
```

### 3. Gestión de Turnos

```csharp
public virtual IEnumerator OnStartTurn()
{
    yield return ActiveEffectsOnStartTurn();
    if (!isDeath)
    {
        foreach (Skill skill in Skills.skills)
        {
            skill.OnStartTurn();
        }
        foreach (Skill skill in Skills.onStartTurn)
        {
            this.ThrowSkill(skill);
            yield return new WaitForSeconds(this.betweenActionsDelay);
        }
        foreach (IOnTurn onTurn in onTurnActionsCopy)
        {
            onTurn.OnStartTurn();
            yield return new WaitForSeconds(this.betweenActionsDelay);
        }
    }
}
```

## Sistema de Habilidades

La clase
 gestiona las habilidades de la unidad:

| Propiedad | Descripción | Uso |
| --- | --- | --- |
| skills | Lista de habilidades | Almacenamiento base |
| onTakeDamage | Habilidades al recibir daño | Efectos defensivos |
| onStartCombat | Habilidades al inicio del combate | Iniciativas |
| onStartTurn | Habilidades al inicio del turno | Acciones automáticas |

## Unidades especiales

### [1. Enemigos](./Creatures.md)

Los enemigos son las criaturas principales del juego, comprenden todos los enemigos, desde los comunes y elites  hasta los jefes.

### [2. Invocaciones](./Summons.md)

Las invocaciones que estan en la zona aliada, son unidades que desaparecen al pasar los turnos.

### [3. Heroes](./Heros.md)

Los personajes seleccionables del juego, que poseen un mazo de cartas especifico cada uno.

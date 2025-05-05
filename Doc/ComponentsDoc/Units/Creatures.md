# Sistema de Criaturas

## Estructura Base

La clase [`Creature`](../../../Assets/src/app/Unit/Creatures/Creature.cs) extiende la funcionalidad base de [`Unit`](../../../Assets/src/app/Unit/Units.cs) para implementar criaturas con comportamiento autónomo. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| level | Nivel de la criatura | Afecta estadísticas base |
| attacks | Lista de ataques | Define capacidades de combate |
| skills | Lista de habilidades | Define capacidades especiales |
| type | Tipo de criatura | Controla comportamiento base |

## Tipos de Criaturas

El sistema implementa varios tipos de criaturas:

| Tipo | Descripción | Uso |
| --- | --- | --- |
| commun | Criatura común | Enemigos básicos |
| event_ | Criatura de evento | Encuentros especiales |
| elit | Criatura élite | Enemigos poderosos |
| towerboss | Jefe de torre | Bosses intermedios |
| finalboss | Jefe final | Bosses principales |
| summon | Invocación | Unidades aliadas temporales |
| any | Cualquiera | Comodín para sistemas generales |

## Sistema de Acciones

La clase implementa un sistema de selección de acciones:

```csharp
public virtual void NextAction()
{
    this.nextAttack = null;
    this.nextSkill = null;
    if (stun)
    {
        (this.prefab as CreatureMono).OnNextAction(null);
        return;
    }
    bool GetNextAttack()
    {
        if (canAttack && (UnityEngine.Random.Range(0, 100) <= attackProbability || !canUseSkill))
        {
            this.nextAttack = GetRandomAttack();
            this.nextAttack.RefreshAttackState();
            (this.prefab as CreatureMono).OnNextAction(nextAttack);
            return true;
        }
        return false;
    }
    if (!GetNextAttack() && canUseSkill)
    {
        this.nextSkill = GetRandomSkill();
        if (this.nextSkill)
            (this.prefab as CreatureMono).OnNextAction(nextSkill);
    }
    if (this.nextSkill == null && this.nextAttack == null)
        (this.prefab as CreatureMono).OnNextAction(null);
}
```

## Sistema de Niveles

El sistema implementa un cálculo de estadísticas basado en niveles:

```csharp
[JsonIgnore]
public override int maxHealth =>
    this.level > 1
    ? (int)
    math.ceil(
        maxHealth_
        * math.pow(1 + UnitsConfig.levelIncreseStatsPercentage / 100f, level - 1)
    )
    : maxHealth_;
```

## Sistema de Combate

La clase implementa un sistema de combate autónomo:

```csharp
public override void StartAction()
{
    if (stun || isDeath)
    {
        this.FinalizeAction();
        return;
    }
    base.StartAction();
    if (nextAttack != null)
        this.ThrowAttack(nextAttack);
    if (nextSkill != null)
        ThrowSkill(nextSkill);
    if (nextAttack == null && nextSkill == null)
        this.FinalizeAction();
}
```

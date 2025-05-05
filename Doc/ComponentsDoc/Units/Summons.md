# Sistema de Invocaciones

## Estructura Base

La clase [`Summon`](../../../Assets/src/app/Unit/Summons/Summon.cs) extiende la funcionalidad base de `Unit` para implementar unidades invocadas temporales. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| owner | Dueño de la invocación | Control de alianza |
| duration | Duración en turnos | Control de vida útil |
| acumulate | Comportamiento de acumulación | Control de mezcla con otras invocaciones |


## Sistema de Creación

La clase implementa dos métodos de creación:

```csharp
public Summon CreateInstance(Unit owner, int duration)
{
    Summon instance = this.CreateInstance() as Summon;
    instance.owner = owner;
    instance.duration = duration;
    return instance;
}
```

## Sistema de Mezcla

El sistema implementa un mecanismo de acumulación de invocaciones:

```csharp
public void Merge()
{
    if (this.acumulate)
    {
        bool foundSimilar = false;
        foreach (Unit unit in owner.team.units)
        {
            if (unit.id == this.id && unit is Summon)
            {
                (unit as Summon).duration += this.duration;
                foundSimilar = true;
                Destroy(this.prefab.gameObject);
                break;
            }
        }
        if (!foundSimilar)
            this.InvokeOnTeam(owner.team);
    }
    else
        this.InvokeOnTeam(owner.team);
    }
}
```

## Ciclo de Vida

El sistema implementa un ciclo de vida controlado por turnos:

```csharp
public override void OnLateEndTurn()
{
    this.duration -= 1;
    if (this.duration <= 0)
        this.OnDeath();
}
```

# IOnDamage
El [`interface IOnDamage`](../../../Assets/src/app/Actions/Unit%20Actions/OnDamage/IOnDamage.cs) se utiliza para el momento en que se recibe daño. Si un efecto, skill, item u otra entidad hereda de `IOnDamage`, entonces debe implementar el método `OnDamage(Unit target, Damage damage)`. Este método `OnDamage` es usado tanto para daño realizado, como para daño recibido. A continuación se explica su caso de uso en el proyecto.
```csharp
public interface IOnDamage : IUnitAction
{
    // Prioridad al momento de activar la acción de los IOnDamage.
    public int onDamagePriority { get; set; }
    public void OnDamage(Unit target, Damage damage);
}
```
Es importante tener en cuenta que estos interfaces se usan cuando se recibe daño, cuando se recibe un impacto y debe controlar el daño que se está ejerciendo. No se ejecutan en el momento de lanzar el ataque, sino en el momento de impacto.

## IOnTakeDamage
El [`interface IOnTakeDamage: IOnDamage`](../../../Assets/src/app/Actions/Unit%20Actions/OnDamage/IOnTakeDamage.cs) es usado para el momento en que se recibe daño. Las clases que hereden de este interfaz están obligadas a implementar `void OnTakeDamage(Unit target, Damage damage)` y el método heredado de `IOnDamage`, `void OnDamage(Unit target, Damage damage)`.
```csharp
public interface IOnTakeDamage : IOnDamage
{
    public void OnTakeDamage(Unit target, Damage damage);
}
```

### Ejemplo
Por ejemplo, una unidad tiene el efecto [`Intangible`](../../../Assets/src/app/Effects/buffs/Intangible.cs), este efecto hereda de `IOnTakeDamage`, entonces en el momento de recibir daño una unidad con este efecto, pasará por el método implementado por intangible y modificará el daño que está actuando sobre la unidad.
```csharp
public void OnDamage(Unit target, Damage damage)
{
    OnTakeDamage(target, damage);
}
public void OnTakeDamage(Unit target, Damage damage)
{
    this.Execute();
    damage.damage = 0;
}
```

## IOnApplyDamage
El [`interface IOnApplyDamage:IOnDamage`](../../../Assets/src/app/Actions/Unit%20Actions/OnDamage/IOnApplyDamage.cs) actúa muy parecido a `IOnTakeDamage`, la diferencia es que este es usado al aplicar un ataque, al realizar un golpe.
```csharp
public interface IOnApplyDamage : IOnDamage
{
    public void OnApplyDamage(Unit target, Damage damage);
}
```

### Ejemplo
Por ejemplo, el efecto [`Debil`](../../../Assets/src/app/Effects/debuffs/Weak.cs), en el momento de atacar, debe disminuir en un $25$% el daño realizado. Es, en este caso particular, donde es llamado su método heredado `OnApplyDamage(Unit target, Damage damage)`
```csharp
public void OnDamage(Unit target, Damage damage)
{
    OnApplyDamage(target, damage);
}
public void OnApplyDamage(Unit target, Damage damage)
{
    damage -= (int)math.ceil(damage.damage * (damageDecresePercentage / 100f));
    damage.damage = math.max(damage, 0);
    this.Execute();
}
```

## Caso de uso
### Prioridad de activación
Para comprender el caso de uso en el código de estas acciones, es necesario conocer cómo está pensado que actúe la prioridad de activación.
El concepto `prioridad` se usa para establecer el orden de ejecuciones. Esta prioridad se usa para establecer el orden en que se activan las acciones [`IOnDamage`](./ComponentsDoc/Interfaces/IOnDamage.md). Por ejemplo, cuando una entidad es `Intangible`, esta reduce el daño a cero, pero nosotros queremos devolver el daño con un efecto `Reflexión`, es entonces prioritario entrar primero a reflexión antes de reducir el daño a cero.
```csharp
//En este ejemplo podríamos tener algo como
Reflexion.onDamagePriority => 0;
Intangible.onDamagePriority => 1;
```
De esta forma primero se ejecutará `Reflexión` y luego `Intangible` dado que `Reflexion.onDamagePriority < Intangible.onDamagePriority`.

### Código usado
El uso de este interfaz se ve principalmente en la clase [`Damage`](../../../Assets/src/app/Attack/Damage.cs), este es el componente que se encarga de controlar el daño y el método para ello actúa llamando cada uno de los `IOnDamage` y aplicando su efecto.
```csharp
private void OnApplyDamageInvoke(Unit unit)
{
    //TODO: Hacer esto aquí implica un costo computacional de 2*N + Sort, cuando el costo debería ser N + insert, luego el orden en O(N) igual, y en poca cantidad de elementos. Ergo, se prioriza la limpieza del código en este caso.
    Dictionary<int, List<IOnDamage>> actions = new Dictionary<int, List<IOnDamage>>();
    foreach (IOnApplyDamage onApplyDamage in onApplyDamageActions)
        if (!actions.ContainsKey(onApplyDamage.onDamagePriority))
            actions.Add(
                onApplyDamage.onDamagePriority,
                new List<IOnDamage>() { onApplyDamage }
            );
        else
            actions[onApplyDamage.onDamagePriority].Add(onApplyDamage);
    foreach (IOnTakeDamage onTakeDamage in unit.onTakeDamageActions)
        if (!actions.ContainsKey(onTakeDamage.onDamagePriority))
            actions.Add(onTakeDamage.onDamagePriority, new List<IOnDamage>() { onTakeDamage });
        else
            actions[onTakeDamage.onDamagePriority].Add(onTakeDamage);
    List<int> orderExecution = actions.Keys.ToList();
    orderExecution.Sort();
    foreach (int key in orderExecution)
    {
        foreach (IOnDamage action in actions[key])
        {
            //Si el ataque es reflejado no debería pasar nada de este
            if (this.isReflected)
                break;
            action.OnDamage(unit, this);
        }
    }
}
```

## Arquitectura en código
```
├──assets/src/app
|            ├──Actions/
|            |   ├── UnitActions/
|            |   │   ├── OnDamage/
|            |   │   |   ├── OnDamage.cs
|            |   │   |   ├── OnTakeDamage.cs
|            |   │   |   └── OnApplyDamage.cs
```
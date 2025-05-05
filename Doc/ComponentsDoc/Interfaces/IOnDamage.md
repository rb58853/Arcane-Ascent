# IOnDamage

El `interface IOnDamage` se utiliza para el momento en que se recibe danno. Si un efecto, skill, item u otra entidad hereda de `IOnDamage`, entonces debe implementar el metodo `OnDamage()`. Este metodo `OnDamage` es usado tanto para danno realizado, como para danno recibido. A continuacion se explica su caso de uso en el proyecto.

```c#
public interface IOnDamage 
{
    public void OnDamage(Damage damage);
}
```

## IOnTakeDamage

El `interface IOnTakeDamage: IOnDamage` es usado para el momento en cual se recibe danno. Por ejemplo, una unidad tiene el efecto [`Intangible`](TODO), este efecto hereda de `IOnTakeDamage`, entonces en el momento de recibir danno una unidad.

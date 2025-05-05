# Sistema de Héroes

La clase [`Hero`](../../../Assets/src/app/Unit/Heros/Hero.cs) extiende la funcionalidad base de `Unit` para implementar héroes jugables. Esta implementación permite:

| Característica | Descripción | Uso |
| --- | --- | --- |
| mana | Sistema de recursos | Control de uso de cartas |
| deck | Mazo de cartas | Gestión de cartas disponibles |
| inventary | Inventario | Gestión de items y reliquias |
| avatarImage | Imagen de perfil | Identificación visual |

## Sistema de Recursos

El sistema implementa dos tipos principales de recursos:

### 1. Mana

```csharp
[field: SerializeField, Min(1)]
[JsonRequired]
public int MaxMana { get; protected set; } = 3;
[JsonRequired]
public int mana { get; protected set; }
public int maxMana
{
    get
    {
        if (needUpdateMaxMana)
        {
            endMaxMana = MaxMana;
            foreach (IOnMaxMana onMaxMana in onMaxManaList)
                endMaxMana += onMaxMana.maxManaVariation;
            needUpdateMaxMana = false;
        }
        return endMaxMana;
    }
}
```

### 2. Oro

```csharp
[field: SerializeField, Space(20), Header("Gain Gold")]
[JsonIgnore]
GameObject burstGainGold;
[SerializeField]
[JsonIgnore]
OnHitText textGold;
public int GainGold(int gold)
{
    if (this.prefab_ != null && this.prefab_.isActiveAndEnabled)
    {
        if (burstGainGold != null)
        {
            MonoBehaviour.Instantiate(burstGainGold, this.prefab.transform);
            OnHitText textGold = MonoBehaviour.Instantiate(this.textGold);
            textGold.Initialize(gold, this.prefab.gameObject, health: true);
        }
    }
    int recived = gold > 0 ? gold : 0;
    this.gold += recived;
    GoldText.UpdateGoldText();
    return recived;
}
```

## Sistema de Cartas

El sistema implementa un manejo completo de cartas:

```csharp
[field: SerializeField, Tooltip("Todas las cartas que puede tener el heroe")]
[JsonProperty(NullValueHandling = NullValueHandling.Ignore, IsReference = false)]
public DeckCards fullCards { get; private set; }
[field: SerializeField, Tooltip("Todas las cartas desbloqueadas para este heroe")]
[JsonProperty(NullValueHandling = NullValueHandling.Ignore, IsReference = false)]
public DeckCards unlockedCards { get; private set; }
[field: SerializeField]
[JsonRequired]
int maxCardsInHand { get; set; } = 6;
```

## Sistema de Inventario

La clase implementa un sistema de inventario integrado:

```csharp
[field: Space(30), Header("Inventary"), SerializeField]
[JsonRequired]
public Inventary inventary { get; private set; }
public void Buy(Item item)
{
    this.inventary.items.Add(item);
    this.gold -= item.price;
}
public void Buy(Card card)
{
    this.deck.Add(card);
    this.gold -= card.price;
}
```

## Ciclo de Combate

El sistema implementa un ciclo de combate completo:

```csharp
public override void OnStartCombat()
{
    base.OnStartCombat();
    this.onPlayCard = null;
    this.inventary.OnStartCombat();
    this.deck.OnStartCombat();
    this.mana = this.maxMana;
}
public override void OnEndCombat()
{
    base.OnEndCombat();
    foreach (Effect effect in new List<Effect>(this.buffs.Values))
        effect.OnEndCombat();
    foreach (Effect effect in new List<Effect>(this.debuffs.Values))
        effect.OnEndCombat();
}
```

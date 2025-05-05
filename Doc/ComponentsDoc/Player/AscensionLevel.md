# Niveles de ascension

La clase [`AscensionLevel`](../../../Assets/src/app/Player/AscensionLevel/AscensionLevel.cs) representa un nivel de ascensión en el juego, manejando las modificaciones de dificultad y características especiales de cada nivel. Esta clase es serializable y utiliza referencias JSON para persistencia de datos.

## Características Principales

| Característica | Descripción |
| --- | --- |
| Nivel | Identifica el nivel de ascensión actual |
| Icono | Representación visual del nivel |
| Nivel de Enemigos | Define la dificultad de los enemigos |
| Restauración de Salud | Controla la cantidad de salud recuperada en zonas de descanso |
| Oro Obtenido | Modifica el porcentaje de oro recibido |
| Pérdida Inicial | Define la salud perdida al iniciar la partida |
| Reducción Máxima | Limita la pérdida máxima de salud |
| Restauración por Jefe | Define la curación recibida al derrotar jefes |
| Jefe Final | Referencia al jefe final del nivel |

## Propiedades Principales

### level

- Tipo: int
- Descripción: Número del nivel de ascensión
- Acceso: get/set
- Valor por defecto: 0
- Uso: Identificador único del nivel

### icon

- Tipo: Sprite
- Descripción: Icono visual del nivel
- Acceso: get/set
- Uso: Representación gráfica en la interfaz

### enemiesLevel

- Tipo: int
- Descripción: Nivel de dificultad de los enemigos
- Acceso: get/set
- Valor por defecto: 1
- Rango: 1 o mayor
- Uso: Escala la dificultad de los enemigos

### healtRestoreInRestoreZone

- Tipo: int
- Descripción: Porcentaje de salud restaurada en zonas de descanso
- Acceso: get/set
- Valor por defecto: 100
- Rango: 0-100
- Uso: Modifica la eficacia de las zonas de curación

### obtainGoldPercentage

- Tipo: int
- Descripción: Porcentaje de oro obtenido
- Acceso: get/set
- Valor por defecto: 100
- Rango: 0-100
- Uso: Controla la recompensa de oro

### initHealtLossPercentage

- Tipo: int
- Descripción: Porcentaje de salud perdida al iniciar
- Acceso: get/set
- Valor por defecto: 0
- Rango: 0-100
- Uso: Define la penalización inicial de salud

### maxHealthReduction

- Tipo: int
- Descripción: Reducción máxima de salud
- Acceso: get/set
- Valor por defecto: 0
- Rango: 1 o mayor
- Uso: Limita la pérdida total de salud

### HealthRestoreOnBossDefeat

- Tipo: int
- Descripción: Porcentaje de salud restaurada al derrotar jefes
- Acceso: get/set
- Valor por defecto: 100
- Rango: 0-100
- Uso: Define la recompensa de salud por victoria

### finalBoss

- Tipo: FinalBoss
- Descripción: Referencia al jefe final del nivel
- Acceso: get/set
- Uso: Define el desafío final del nivel

## Operadores Especiales

### Operador de Conversión Explícito

```csharp
public static explicit operator AscensionLevel(int index)
```

- Convierte un índice a un objeto AscensionLevel
- Utiliza el controlador de niveles de ascensión del jugador

### Operador de Conversión Implícito

```csharp
public static implicit operator int(AscensionLevel ascensionLevel)
```

- Convierte un AscensionLevel a su nivel numérico
- Retorna el valor de la propiedad level

## Método ToString()

El método `ToString()` genera una descripción formateada de las modificaciones del nivel, incluyendo:

- Restauración de salud en derrota de jefes
- Restauración en zonas de descanso
- Nivel de enemigos
- Reducción de oro
- Pérdida inicial de salud
- Reducción máxima de salud

# Notas

- Todas las propiedades son privadas para el setter excepto level
- Los valores tienen rangos específicos para mantener la consistencia del juego
- El método `ToString()` utiliza el sistema de idiomas para la localización

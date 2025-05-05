# Sistema Puertas

La clase [`Door`](../../../../Assets/src/app/Tower/Door/Door.cs) es una clase serializable que maneja la lógica de las puertas en el juego. Cada puerta tiene un elemento asociado, un nivel de rareza y puede ser especial (tienda, zona de restauración, etc.). Esta clase es fundamental para el sistema de combate y progresión del juego.

## Características Principales

| Característica | Descripción |
| --- | --- |
| Elemento | Cada puerta tiene un elemento asociado que afecta al combate |
| Rareza | Define el tipo y dificultad de los enemigos que aparecerán |
| Especial | Puede ser una tienda, zona de restauración o puerta normal |
| Nivel | Determina la potencia de los enemigos basándose en el piso |

<!-- Referencia de Propiedades
------------------------ -->

## Propiedades Principales

### element

- Tipo: Element
- Descripción: Elemento asociado a la puerta
- Acceso: get/set protegido
- Uso: Determina el elemento de los enemigos y afecta al combate

### rarity

- Tipo: CreatureType
- Descripción: Rareza de los enemigos que aparecerán
- Acceso: get/set protegido
- Uso: Define la dificultad del combate

### floor

- Tipo: Floor
- Descripción: Piso al que pertenece la puerta
- Acceso: get/set protegido
- Uso: Determina el nivel de poder de los enemigos

### specialDoor

- Tipo: SpecialDoor
- Descripción: Tipo especial de puerta
- Acceso: get/set
- Uso: Determina si es una tienda, zona de restauración o puerta normal

## Propiedades Derivadas

### powerLevel

- Tipo: int
- Descripción: Nivel de poder basado en el piso
- Acceso: get
- Uso: Determina la potencia de los enemigos

# Métodos Principales

## Métodos de Combate

### CombatInto

- Tipo: Combat
- Descripción: Combate interno de la puerta
- Acceso: get
- Uso: Maneja el combate cuando se entra por la puerta normal

### EndCombat()

- Descripción: Limpia los datos del combate después de terminar
- Uso: Libera recursos cuando el combate ha finalizado

## Métodos de Acción

### Enter()

- Descripción: Maneja la entrada a través de la puerta
- Uso: Inicia el combate o carga la escena especial según el tipo de puerta

# Sistema de Enemigos

La clase implementa un sistema complejo de generación de enemigos basado en la rareza:

### CommunDoor

- Genera múltiples enemigos comunes
- Mantiene un balance de elementos
- Ajusta los niveles de poder
- Garantiza una distribución mínima del elemento de la puerta

### ElitDoor

- Genera un enemigo elite
- Verifica la disponibilidad del elemento
- Mantiene un porcentaje mínimo de criaturas del elemento de la puerta

### TowerBossDoor

- Genera un jefe de torre
- Valida la disponibilidad del elemento
- Selecciona un enemigo aleatorio del elemento especificado

### FinalBossDoor

- Genera el jefe final
- Crea una instancia del boss basada en el nivel de ascensión

# Notas

- La clase utiliza un sistema de serialización para persistencia de datos
- Implementa un manejo eficiente de recursos con limpieza de combates
- El sistema de enemigos es escalable y configurable
- La lógica de generación de enemigos es diferente según la rareza

## TODO
- Optimizar el uso de los `get` en las clases, dado que la complejidad temporal de esto es alta cada vez que solo se llama a la propiedad!! PAra esto tengo que crear un initializador en  cada clase y manejar manualmente la insercion de nuevos items.
- Todo lo que sea lanzar skills y ataques que pase a ser en forma de cola. De esta forma no se puede lanzar un ataque o skill si no llega tu turno en la cola. Actualmente se pueden lanzar las cartas del heroe sin pausa. Lo proximo que se hara sera controlar si puede activarse una carta o no. Pero en un futuro lo mejor es formar una cola de cartas en caso que se tiren consecutivamente, incluso visualmente que se vea esto.
- Las skills se guardan en cola a la hora de lanzarse. Para que no se acumule esa parte visual.
- Separar las habilidades en tipos de habilidades. Para no tener informacion de sobra en el inspector. Lo mismo para ataques o lo que requiera esto
- Reescalar todo el juego de forma que las critauras usen 1 como escala. Esta en 0.2 xq el anterior dev tenia asi los assets y yo pensaba reutilizarlos
- Los ataques deben setear una variable `Damage` o `baseDamage` para hacer que `damage` dependa directamente de estas y no hacerlo durante el init. 
- Grid de los items activavbles no esta escalable segun resolucion
- Grid de los items de evento no esta escalable segun resolucion
- Que las cartas no se instancien solo para verlas en el container. En vez de usar el `card.prefab` usar `card.Prefab`.
- El arte esta desigual. Hay que hacer que todo el arte sea lo mismo. Estoy usando paneles de un arte y de otro y queda mal.ss
- Problema con las distancias de los drop en los eventos.

## Future
- Refactoriar todas las clases y particionarlas en componentes. Por ejemplo un componente que se encarga de todo lo que sea recibir danno, otro de realizar acciones, otro de morirse. No esta mal lo existente. Pero la organizacion puede mejorar
<!-- - Estaria muy bien crear un `ITimes`  o `IMoment` que recoja todos los times del juego, como es IOnStartTurn -->
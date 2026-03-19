# Unidad 5
## Bitácora de proceso de aprendizaje

**ACTIVIDAD 1**




**ACTIVIDAD 2**

1. ¿Qué responsabilidades que antes estaban en draw() ahora están dentro de la clase Emitter?


2. ¿Cuál es la ventaja de encapsular la lógica de emisión en una clase separada?


3. En este ejemplo hay un array de emitters. ¿Quién crea los emitters? ¿Quién crea las partículas dentro de cada emitter?


4. Dibuja un diagrama que muestre la jerarquía: sketch → [emitters] → [partículas]. ¿Cuántos niveles de “colección” hay?
   

**ACTIVIDAD 3**

1. ¿Qué tienen en común las subclases de partículas? ¿Qué tienen de diferente?

Las subclases de las particulas tienen en común todos los atributos y todos los métodos menos el _Show()_. En el _Show()_ estas subclases tienen comportamientos diferentes, ya que uno pinta circulos y otro cuadrados y de maneras diferentes.

2. ¿Por qué es importante que el Emitter no necesite saber qué tipo específico de partícula está gestionando? Explica esto con tus propias palabras.

Porque nos permire tratar de manera polimorfica las particulas, sin tener que preguntar especificamentente si es una instancia de confetti o de particle. También por otro lado está la ventaja de escalabilidad y desacoplamiento.

3. Si mañana quisieras agregar un tercer tipo de partícula, ¿Qué tendrías que crear y qué NO tendrías que modificar?

Tendría que crear una nueva clase que herede de particle, no tendría que modificar nada ni en la capa del emitter, ni en la del sketch. Tampoco tengo que cambiar nada en los atributos o métodos del nuevo tipo de particula que cree a no ser de que quiera hacer polimorfismo.

4. Compara con Example 4.2: ¿Cambió la lógica del Emitter? ¿Cambió la lógica de muerte? ¿Qué capa del sistema se modificó y cuáles permanecieron intactas?


**ACTIVIDAD 4**




## Bitácora de aplicación 


## Bitácora de reflexión

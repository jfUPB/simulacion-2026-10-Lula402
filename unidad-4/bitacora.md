# Unidad 4

## Bitácora de proceso de aprendizaje

**ACTIVIDAD 1**
Los aspectos que más me llamaron la atención de la obra de Memo, son los siguientes:

- Leyendo un poco más, me interesó mucho como las cues se daban via in-ear monitors, eso me hace recordar un poco a la unidad de random, no por random, sino porque es ese toque de imperfección en algo que puede ser perfecto. Me gusta mucho que se da claramente una especie de delay mientras escuchan la cue y la realizan, eso lo hace humano, lo hace mejor.
- Me llamó la atención como partes de la performance que uno logra escuchar sincronia y orden, pero en cualquier momento todo el performance vuelve a caos y a que pareces un montón de personas golpeando sin estar concectadas a la otra.
- Por último, me llamó la atención, que el en la documentación dice que los drummers golpean en "fixed time intervals", entonces no solo se hace interesante por la concentración de los performers, sino, por lo especial que es darse cuenta que todos juntos suenan imperfectos, pero individualmente son matemáticamente perfectos por el intervalo.

**ACTIVIDAD 2**

1. ¿Qué está pasando en esta simulación? ¿Cuál es la interacción?

En esta simulación, la bara con las bolitas está girando constantemente. El detallito es que no es que tenga velocidad o un motion 101, sino que su rotación esta determinada por una variable angle, y como esta se aumenta gradualmente en el draw, da el efecto que el cosito está girando a una velocidad constante. En cuando a la interación, al presionar una tecla al angulo se le suma +0.1, esto lo que hace es que pegue un mini brinquito hacia la diracción en la que va, pero es super poco perceptible, debido a que el avance es el mismo que el del movimiento original.

2. P5 por defecto pone el sistema de coordenadas en la esquina superior izquierda, entonces si hicieramos esta translación solo una vez, el sistema regresaría a esa posicón. Se translada entonces al centro en cada frame antes de rotar y de pintar la figura, asi es el orden lógico. Me hace recordar a cuando hacemos la aceleración = 0 para resetear y empezar en limpio.

3. El rotate() siempre va a girar respecto a la posición (0,0) del sistema, entonces por eso lo necesitamos seteado en el centro antes de calcular la rotación.

4. Al dibujar los elementos gráficos se dibujan en la posición (0, 0) del sistema de coordenadas porque es mucho más sencillo dibujarlos donde deben ir una sola vez. Aunque en cada frame se hace lo mismo, la figura parece rotar porque es el sistema coordenado el que está rotando. Me explico, cuando hacemos translate estamos haciendo translate al sistema, cierto? entonces cuando hacemos rotate, lo que estamos rotando también es el sistema. 

Imaginemos que el canvas es una hoja de papel, si yo no hiciera translate, entonces estaría anclando la punta superior izquierda de la hoja y girandola, pero a hacer el translate, estoy anclando la hoja en su centro y girandola sobre si misma. De esa manera, aunque la figura no es quien se mueva, parece hacerlo.

**ACTIVIDAD 3**
**ACTIVIDAD 4**
**ACTIVIDAD 5**
**ACTIVIDAD 6**
## Bitácora de aplicación 



## Bitácora de reflexión

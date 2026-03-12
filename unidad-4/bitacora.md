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

5. En este ejemplo, no hay función de appply force, pero dentro del update del mover está el 101. Ahí se encuentra el calculo de la posición del mouse y la creación del vector dir que resta la posición del mouse y la de mover, para así perseguirlo.


6. ¿Qué hace la función heading()?
   
Esa función probablemente está dejando de lado la magnitud de la velocidad, para fijarse solamente en la dirección y retornarla como un numero, para que pueda ser usada como ángulo.

7. ¿Qué hace la función push() y pop()? 
   
Permiten que podamos mover y rotar el canvas solo para ese objeto con el que estamos tratando. El push() es como si tomara una foto o se memorizara como estaban las cosas, luengo dentro del bloque se hacen cambios, y por último con el pop ya terminamos con este objeto y se borra todos los cambios que acabamos de hacer para volver a como estabamos al hacer el push.

8. ¿Qué hace rectMode(CENTER)? 

Esto usa la coordenada (x,y) que tiene el objeto, como el centro de la rect. Si es rectMode(CENTER) y decimos rect(20,20,width, height) entonces la mitad de esta rect va a estar ubicada en el (20,20) del canvas.

9. ¿Cuál es la relación entre el ángulo de rotación y el vector de velocidad? Trata de dibujar en un papel el vector de velocidad y cómo se relaciona con el ángulo de rotación y la operación de traslación y rotación.
    
**-> Estado Inicial:** El origen (0,0) está arriba a la izquierda.

**-> translate(pos.x, pos.y):** El origen se mueve a donde está el objeto. Ahora el rect esta en su propio sistema donde su centro es el (0,0).

**-> rotate(velocity.heading()):** El sistema rota hasta que el eje X se inclina para alinearse perfectamente con el vector de velocidad.

**-> rect(0, 0, 30, 10):** Dibujamos el rect sobre ese eje X ya inclinado.

**ACTIVIDAD 3**

**LINK**
https://editor.p5js.org/Lula402/full/WxC5-7KYO

**CÓDIGO**

vehiculo.js
```js
class Vehiculo {
  constructor() {
    this.posicion = createVector(width / 2, height / 2);
    this.velocidad = createVector(0, 0);
    this.aceleracion = createVector(0, 0);
    this.r = 15; 
    this.topSpeed = 5;
  }

  applyForce(fuerza) {
    this.aceleracion.add(fuerza);
  }

  update() {
    this.velocidad.add(this.aceleracion);
    this.velocidad.limit(this.topSpeed);
    this.posicion.add(this.velocidad);
    this.aceleracion.mult(0);
    this.velocidad.mult(0.98);
  }

  display() {

    let angulo = this.velocidad.heading();

    push();
    translate(this.posicion.x, this.posicion.y);
    rotate(angulo);  
    fill(127);
    stroke(0);
    strokeWeight(2);
    triangle(this.r, 0, -this.r, -this.r/2, -this.r, this.r/2);
    pop();
  }
}
```
Scketch.js
```js
let v;

function setup() {
  createCanvas(600, 400);
  v = new Vehiculo();
}

function draw() {
  background(240);

  if (keyIsDown(LEFT_ARROW)) {
    let fuerzaIzquierda = createVector(-0.1, 0);
    v.applyForce(fuerzaIzquierda);
  }
  if (keyIsDown(RIGHT_ARROW)) {
    let fuerzaDerecha = createVector(0.1, 0);
    v.applyForce(fuerzaDerecha);
  }

  v.update();
  v.display();
  
  checkEdges(v);
}

function checkEdges(obj) {
  if (obj.posicion.x > width) obj.posicion.x = 0;
  if (obj.posicion.x < 0) obj.posicion.x = width;
}
```

**ACTIVIDAD 4**



**ACTIVIDAD 5**
**ACTIVIDAD 6**
**ACTIVIDAD 7**
**ACTIVIDAD 8**
**ACTIVIDAD 9**
**ACTIVIDAD 10**
## Bitácora de aplicación 



## Bitácora de reflexión



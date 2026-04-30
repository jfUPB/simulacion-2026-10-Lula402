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

1.  ¿Qué modificación hay que hacer al motion 101 cuando se quiere agregar fuerzas acumulativas?

La modificación es muy simple, sería solo quitar la línea donde reseteamos la aceleración y decimos acc.mult(0), al quitarla ya quedamos con fuerzas acumulativas porque en cada frame ya no empezamos sin fuerzas y a calcular desde 0, sino que las fuerzas del frame anterior van a estar ahí y van a afectar en los nuevos cálculos.

2.

<img width="221" height="155" alt="image" src="https://github.com/user-attachments/assets/e8399932-8059-4424-be27-50c7ed346054" />

3.

![attractor](https://github.com/user-attachments/assets/08254e72-c9ec-413c-847b-91e4f34ad782)

ATTRACTOR
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Attractor {
  constructor() {
    this.position = createVector(width / 2, height / 2);
    this.mass = 20;
    this.G = 1;
    this.dragging = false;
    this.rollover = false;
  }

  attract(mover) {
    // Calculate direction of force
    let force = p5.Vector.sub(this.position, mover.position);
    // Distance between objects
    let distance = force.mag();
    // Limiting the distance to eliminate "extreme" results for very close or very far objects
    distance = constrain(distance, 5, 25);

    // Calculate gravitional force magnitude
    let strength = (this.G * this.mass * mover.mass) / (distance * distance);
    // Get force vector --> magnitude * direction
    force.setMag(strength);
    return force;
  }

  // Method to display
  display() {
    ellipseMode(CENTER);
    stroke(0);
    if (this.dragging) {
      fill(110, 20, 200);
    } else if (this.rollover) {
      fill(0);
    } else {
      fill(220, 240, 10);
    }
    ellipse(this.position.x, this.position.y, this.mass * 2);
  }

  handleRollover(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.rollover = true;
    } else {
      this.rollover = false;
    }
  }

  handlePress(mx, my) {
    let d = dist(mx, my, this.position.x, this.position.y);
    if (d < this.mass) {
      this.dragging = true;
    }
  }

  handleRelease() {
    this.dragging = false;
  }

  handleDrag(mx, my) {
    if (this.dragging) {
      this.position.x = mx;
      this.position.y = my;
    }
  }
}
```
SKETCH
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let movers = [];
let attractor;

function setup() {
  createCanvas(640, 240);

  for (let i = 0; i < 20; i++) {
    movers.push(new Mover(random(width), random(height), random(0.1, 2)));
  }
  attractor = new Attractor();
}

function draw() {
  background(255);
  attractor.handleRollover(mouseX, mouseY);
  attractor.handleDrag(mouseX, mouseY);  
  attractor.display();

  for (let i = 0; i < movers.length; i++) {
    let force = attractor.attract(movers[i]);
    movers[i].applyForce(force);

    movers[i].update();
    movers[i].show();
  }
}

function mousePressed() {
  attractor.handlePress(mouseX, mouseY);
}

function mouseReleased() {
  attractor.handleRelease();
}
```

**ACTIVIDAD 5**

1. ¿Cuál es la relación entre r y theta con las posiciones x y y?
   
x = r * cos(theta)

y = r * sin(theta)

La relación esta en que si dibujo un triángulo rectángulo desde el centro, r es la hipotenusa. El cos me da el lado X y el sen la del lado opuesto que es Y.

2. Esta creando un p5 vector **v**, pero este vector lo esta creando a partir solo de el angulo theta, es decir, no tiene magnitud, es 1 (unitario), ahi solo está importando la dirección. Al no entregarle ese segundo parámetro *r*, la bolita parece palpitando en el centro.

3. El movimiento vuelve a la normalidad, el círculo se mueve alrededor del centro, exactamente igual que en el primer código. Todo esto debido a que ya tiene otra vez r, entonces se le da la dirección y la magnitud.

**ACTIVIDAD 6**

<p align=center>
<img width="234" height="188" alt="image" src="https://github.com/user-attachments/assets/935f7bd6-4e0f-4c5a-af32-53a287541fe4" /> </p>

**LINK**

https://editor.p5js.org/Lula402/full/H2S7G_ePT

**CÓDIGO**

```js
//By Lula

let angulo = 0;
let amplitud = 150;
let velocidadAngular = 0.05; 

function setup() {
  createCanvas(600, 400);
  colorMode(RGB, 255, 255, 255, 100);
}

function draw() {
  background(0,0,0,10);
  translate(width / 2, height / 2); 
  let x = amplitud * sin(angulo);
  let y = amplitud * sin(angulo);
  stroke(0);
  fill(180,180,180,50);
  circle(x, 0, 48);
  circle(0, y, 48);
  angulo += velocidadAngular;
}
```

**ACTIVIDAD 7**

<p align=center>
<img width="254" height="162" alt="image" src="https://github.com/user-attachments/assets/4b409de6-f120-4faf-9fd9-60eb151af1ee" /> </p>

**LINK**

[https://editor.p5js.org/Lula402/full/H2S7G_ePT](https://editor.p5js.org/Lula402/full/0Pesy4xRM)

**CÓDIGO**

OSCILLATOR
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

class Oscillator {
  constructor() {
    this.angle = createVector();
    this.angleVelocity = createVector(0,0);
    this.angleAcceleration = createVector(0,0);
    let xAmp = randomGaussian(100, 50); 
    let yAmp = randomGaussian(100, 50);
    this.amplitude = createVector(xAmp, yAmp);
    this.mass = randomGaussian(1, 2); 
  }
  
  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.angleAcceleration.add(f);
  }

  update() {
    this.angleVelocity.add(this.angleAcceleration);
    this.angle.add(this.angleVelocity);
    this.angleVelocity.mult(0.98); 
    this.angleAcceleration.mult(0);
  }

  show() {
    let x = sin(this.angle.x) * this.amplitude.x;
    let y = sin(this.angle.y) * this.amplitude.y;

    push();
    translate(width / 2, height / 2);
    let colorV = map(this.angleVelocity.mag(), 0, 0.1, 0, 1);
    let from = color(218, 165, 32);
    let to = color(72, 61, 139);
    fill(lerpColor(from, to, colorV));
    stroke(0);
    strokeWeight(2);
    line(0, 0, x, y);
    circle(x, y, this.mass * 16); 
    pop();
  }
}

```
SKETCH
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

// An array of objects
let oscillators = [];

function setup() {
  createCanvas(640, 240);
  // Initialize all objects
  for (let i = 0; i < 10; i++) {
    oscillators.push(new Oscillator());
  }
}

function draw() {
  background(255,255,255,20);
  for (let o of oscillators) {
    if (mouseIsPressed) {
      let wind = createVector(0.002, 0.005); 
      o.applyForce(wind);
    }
    o.update();
    o.show();
  }
}
```

**ACTIVIDAD 8**

![serpiente](https://github.com/user-attachments/assets/a3bac97c-b416-44b8-8b3e-3b32b604eeed)

**Los cambios para lograrlo se basaron en:** 

- usar un startAngle = 0; para que cada circulo tuviera un inicio diferente y progresivo cada frame.  

- En el draw se incrementa el angulo para poder que avancen con _startAngle += 0.02;_ y además se guarda ese angulo recien calculado como el actual para ya usarlo _let currentAngle = startAngle;_.

- al momento de calcular Y, ya no se usa angle sino el angulo actual con el que estamos trabjando: _let y = amplitude * sin(currentAngle);_

- Por último, sumarle al angulo actual la velocidad es lo que realmente hace que se mueva como una ola, porque sino sería como una linea de arriba hacia abajo. Esta linea lo que logra y determina es que tanto desfase hay entre los circulos: _currentAngle += angleVelocity;_

**ACTIVIDAD 9**

**SKETCH**

```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let anchor; 
let bob1; 
let bob2;
let spring1; 
let spring2;
let gravity;

function setup() {
  createCanvas(640, 400);
  anchor = createVector(width / 2, 50);
  gravity = createVector(0, 1); 

  bob1 = new Bob(width / 2, 120);
  bob2 = new Bob(width / 2, 220);

  spring1 = new Spring(anchor.x, anchor.y, 100); 
  spring2 = new Spring(bob1.position.x, bob1.position.y, 100);
}

function draw() {
  background(255);

  spring1.connect(bob1);
  spring1.constrainLength(bob1, 30, 200);
  spring2.anchor.set(bob1.position); 

  let force2 = calculateSpringForce(spring2, bob2);
  bob2.applyForce(force2); 
  
  let reactionForce = force2.copy().mult(-1);
  bob1.applyForce(reactionForce);
  spring2.constrainLength(bob2, 30, 200);

  bob1.applyForce(gravity);
  bob1.handleDrag(mouseX, mouseY);
  bob1.update();
  
  bob2.applyForce(gravity);
  bob2.handleDrag(mouseX, mouseY);
  bob2.update();

  spring1.showLine(bob1);
  spring2.showLine(bob2);
  

  spring1.show(); 
  bob1.show();
  bob2.show();
}

function calculateSpringForce(s, b) {
  let f = p5.Vector.sub(b.position, s.anchor);
  let d = f.mag();
  let stretch = d - s.restLength;
  f.setMag(-1 * s.k * stretch);
  return f;
}

function mousePressed() {
  bob1.handleClick(mouseX, mouseY);
  bob2.handleClick(mouseX, mouseY);
}

function mouseReleased() {
  bob1.stopDragging();
  bob2.stopDragging();
}
```

**ACTIVIDAD 10**

https://editor.p5js.org/Lula402/full/OabaVUetd

sketch
```js
let p1;
let p2;

function setup() {
  createCanvas(640, 400);
  p1 = new Pendulum(width / 2, 0, 120);
  p2 = new Pendulum(p1.bob.x, p1.bob.y, 100);
}

function draw() {
  background(255);

  p1.update();
  p1.drag();
  p1.show();

  p2.pivot.set(p1.bob.x, p1.bob.y);

  p2.update();
  p2.drag();
  p2.show();
}

function mousePressed() {
  p1.clicked(mouseX, mouseY);
  p2.clicked(mouseX, mouseY);
}

function mouseReleased() {
  p1.stopDragging();
  p2.stopDragging();
}
```

## Bitácora de aplicación 

**CONCEPTO**
El usuario tiene el control de la luz, es una antorcha. Este se encuentra rodeado de oscuridad y sombras, si se queda quieto, la oscuridad lo alcanza y lo cubre, si se mueve espanta a las sombras.

**LINK**

https://editor.p5js.org/Lula402/full/JDCOY-fB6

**CODIGO**

sombra.js
```js
class Sombra {
  constructor() {
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.topSpeed = 10;
    this.mass = constrain(randomGaussian(20, 8), 5, 50); 
    this.angle = random(TWO_PI);
    this.angleVel = random(0.02, 0.05);
  }

  applyForce(fuerza) {
    let f = p5.Vector.div(fuerza, this.mass);
    this.acc.add(f);
  }

  update() {
    this.vel.add(this.acc);
    this.vel.limit(this.topSpeed);
    this.pos.add(this.vel); 
    this.vel.mult(0.94);
    this.acc.mult(0);
    
    this.angle += this.angleVel;
  }
  
  show() {
    let pulso = map(sin(this.angle), -1, 1, 0.8, 1.2);
    let tam = this.mass * pulso;
    
    push();
    stroke(0, 0, 0, 30);
    strokeWeight(1);
    line(width/2, height/2, this.pos.x, this.pos.y);
    noStroke();
    fill(20, 20, 20, 80); 
    circle(this.pos.x, this.pos.y, tam * 2);
    pop();
  }
}
```

sketch.js
```js
let sombras = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  for (let i = 0; i < 500; i++) {
    sombras.push(new Sombra());
  }
  colorMode(RGB, 255);
}

function draw() {
  background(100, 100, 100, 15);

  let mousePos = createVector(mouseX, mouseY);
  let mouseSpeed = dist(mouseX, mouseY, pmouseX, pmouseY);

  for (let s of sombras) {
    let light = p5.Vector.sub(s.pos, mousePos);
    let dist = light.mag();

    if (dist < 150) {
      let fuerza = map(dist, 0, 150, 15, 0);
      light.setMag(fuerza);
      s.applyForce(light);
    }

    let centro = createVector(width/2, height/2);
    let retorno = p5.Vector.sub(centro, s.pos);
    retorno.mult(0.001); 
    s.applyForce(retorno);
    s.applyForce(retorno);
    s.update();
    s.show();
    checkEdges(s);
  }
  let colorRojo = color(200, 20, 0); // Rojo
  let colorAmarillo = color(255, 240, 50); // Amarillo

  let ratio = map(mouseSpeed, 0, 20, 0, 1);

  let colorAntorcha = lerpColor(colorRojo, colorAmarillo, ratio);

  noStroke();
  fill(red(colorAntorcha), green(colorAntorcha), blue(colorAntorcha), 150);
  circle(mouseX, mouseY, 35);
  fill(red(colorAntorcha), green(colorAntorcha), blue(colorAntorcha), 50);
  circle(mouseX, mouseY, 60);
}

function checkEdges(s) {
  if (s.pos.x > width) s.pos.x = 0;
  if (s.pos.x < 0) s.pos.x = width;
  if (s.pos.y > height) s.pos.y = 0;
  if (s.pos.y < 0) s.pos.y = height;
}
```

**SS**

<p align= center>
<img width="886" height="367" alt="image" src="https://github.com/user-attachments/assets/a56d7f9c-6f52-4242-8c36-a402b4a2bf44" />
</p>

<p align= center>
<img width="452" height="317" alt="image" src="https://github.com/user-attachments/assets/2df80383-9de0-4296-b838-b462cfe8bb59" />
</p>

## Bitácora de reflexión

<p align= center>
<img width="803" height="275" alt="image" src="https://github.com/user-attachments/assets/5f814744-f37d-4db2-8809-806c1348962b" />
</p>









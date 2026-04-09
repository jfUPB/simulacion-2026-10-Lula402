# Unidad 3

## Bitácora de proceso de aprendizaje

**ACTIVIDAD 1**

Lo que siento se puede resumir muy bien en dos palabras: **abrumada e impotente**. 

El mundo cambió muy rápido con la IA y yo le digo a mis amigas, que justo estudian carreras las cuales la IA no se puede apropiar facilmente, que las envidio, que nosotros como artistas, diseñadores y programadores estamos muy golpeados por esto. Les digo que desearía tuvieran empatía y supieran valorar el trabajo de esas personas.

Yo estudio esto porque yo lo quiero hacer con mis manos y mi cerebro, no quiero que una máquina lo haga por mi. Yo quiero poder salir a trabajar y saber que lo hice yo y no una granja de computadores. Suena hasta irónico, pero yo quiero esforzarme en hacer las cosas, yo quiero "sufrir", mejorar, incluso sentir compentencia por otros, mas no saber que todos usamos IA y que es cuestión de quien promtee mejor. 

Ya poniendome asi más pesonal, me pone triste saber que, aprendo un poco más lento o me cuesta más aprender algunas cosas, y que si no uso IA me voy a quedar detrás de mis compañeros y de la industria.


**ACTIVIDAD 2**

1. ¿Por qué es necesario multiplicar la aceleración por cero en cada frame?
Es necesario porque es la manera de resetear la aceleración, de modo que en cada frame se calcule desde el inicio todas las fuerzas que están afectando al objeto y no hayan valores de frames anteriores interfiriendo con los calculos del motion 101.
Es como cuando a propósito limpiamos el background en cada frame, para empezar con un canvas limpio cada vez.

2. ¿Por qué se multiplica por cero justo al final de update()?
Debe ser justo al final, porque en la línea ```this.velocity.add(this.acceleration);``` estamos actualizando la velocidad al sumarle la aceleración, pero si hacemos ```this.acceleration.mult(0);``` antes de, pues entonces la velocidad va a ser constante. Toca tener en cuenta que si la velocidad es constante la posición tampoco va a verse afectada por esto.

3. ¿Qué ves raro? ¿Qué implica esto?
Esta mal porque al dividir de esa manera lo que estamos haciendo es dividir la force que estamos usando en todo el programa entre 10, pero no podemos hacer eso porque se nos dañan los cálculos. Lo que debemos hacer es usar una estática asi: ```let f = p5.Vector.div(force, 10);``` para que el resultado lo guardemos en f, sin dañar a force.

**ACTIVIDAD 3**

**FRICCIÓN**

https://editor.p5js.org/Lula402/full/_JOzHPjfC

<p align=center><img width="959" height="384" alt="image" src="https://github.com/user-attachments/assets/8c037a44-523b-4b5f-9e2b-bef6077641c6" /></p>

**Resistencia a fluidos**

https://editor.p5js.org/Lula402/full/3uuUFkKPr

<p align=center><img width="959" height="384" alt="image" src="https://github.com/user-attachments/assets/75cd9973-6657-49cd-b45e-bb2ee5d18a1f" /></p>

**Atracción gravitacional**

[https://editor.p5js.org/Lula402/full/3uuUFkKPr](https://editor.p5js.org/Lula402/full/ndKK3Vg3l)

<p align=center><img width="959" height="354" alt="image" src="https://github.com/user-attachments/assets/58bee96b-d82e-4555-b45c-9baf2cbb0dcc" /></p>


## Bitácora de aplicación 

**CONCEPTO**

El concepto de esta obra generativa relata como la vida y los procesos no son lineales, hay ups and downs, y como nos movemos por cada momento de la vida de manera distinta. Para esta obra implementé distribución normal (randomGaussian), apliqué fuerzas externas (el wind de las flechas), fuerzas del ambiente (gravedad) y fuerzas reactivas (el drag de las franjas). El motion 101 fue un elemento clave en el movimiento del mover, que representa el usuario. 

Las franjas son de diferentes tonos y con diferente opacidad, en representación a que no todos los momento de la vida son iguales y en algunos dejamos más rastro que en otros.

En alguna franja la fuerza de drag podría ser tan fuerte que al el mover acercarse con velocidad rebota y no logra cruzar al otro lado. Este efecto se dió por pura exploración, pero decidí conservarlo, ya que iba perfecto con la obra y en la vida real a veces queremos que el tiempo pase rápido por situaciones feas, pero como si o si necesitamos pasar al otro lado, nos toca desacelerar y cruzar a la velocidad que el momento lo requiera.

**CÓDIGO**

```FRANJA.js
class Franja {
  constructor(x, w, hsbColor, fuercitaCoe) {
    this.x = x;
    this.w = w;
    this.c = hsbColor;
    this.coe = fuercitaCoe;
  }

  contains(mover) {
    if (mover.position.x > this.x && mover.position.x < this.x + this.w) {
      return true;
    } else {
      return false;
    }
  }

  update() {}

  show() {
    fill(this.c);
    noStroke();
    rect(this.x, 0, this.w, windowHeight);
  }

  calculateDrag(m) {

    let speed = m.velocity.mag();
    
    // (F = c * v^2)
    let dragMagnitude = this.coe * speed * speed;

    let dragForce = m.velocity.copy();

    dragForce.mult(-1);
    
    if (speed > 0){
    dragForce.setMag(dragMagnitude);
    }
    return dragForce;
  }
}
```


```MOVER.JS
class Mover {
  constructor() {
    this.mass = 1;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.arrows = createVector(width / 2, height / 2);
    this.topspeed = 5;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    stroke(0);
    strokeWeight(2);
    fill(127, 127);
    ellipse(this.position.x, this.position.y, 48, 48);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.velocity.x *= -1;
      this.position.x = 0;
    }
    if (this.position.y > height) {
      this.velocity.y *= -1;
      this.position.y = height;
    } else if (this.position.y < 0) {
      this.velocity.y *= -1;
      this.position.y = 0;
    }
  }
}
```


```SKETCH.JS
let mover;
let wind;
let franjas = [];
let posicionX = 0; 

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  mover = new Mover();
  wind = createVector(0, 0);
  
  for (let i =0; posicionX < width; i++){
    let randomWidth = random(1,150);
    let randomBrigthness = random(0,100);
    let randomAlpha = randomGaussian(2, 1);
    let randomCoe = randomGaussian(0.1, 0.1);
    franjas[i] = new Franja(posicionX, randomWidth, color(0, 0, randomBrigthness, randomAlpha), randomCoe);
    posicionX += randomWidth;
  }
}

function draw() {
  wind.set(0, 0);
  
  for (let i =0; i < franjas.length; i++){
    franjas[i].show();
    if (franjas[i].contains(mover) === true){    
      let drag = franjas[i].calculateDrag(mover);
      mover.applyForce(drag);
    }
  }

  let gravity = createVector(0, 0.1);
  mover.applyForce(gravity);

  if (keyIsDown(LEFT_ARROW)) {
    console.log("LEFT_ARROW");
    wind.add(-0.8, 0);
  }
  if (keyIsDown(RIGHT_ARROW)) {
    wind.add(0.8, 0);
  }
  if (keyIsDown(UP_ARROW)) {
    console.log("UP_ARROW");
    wind.add(0, -0.8);
  }
  if (keyIsDown(DOWN_ARROW)) {
    console.log("DOWN_ARROW");
    wind.add(0, 0.8);
  }
  
  mover.applyForce(wind);
  mover.update();
  mover.show();
  mover.checkEdges();
}
```

**LINK**

https://editor.p5js.org/Lula402/full/GxrUIC_XL

**SS**

<p align = center> <img width="959" height="382" alt="image" src="https://github.com/user-attachments/assets/4b6ffd67-fe99-4e57-ba7a-15da12a45bf6" /></p>

<p align = center> <img width="959" height="383" alt="image" src="https://github.com/user-attachments/assets/b6131f0a-4139-4044-8bfe-6d01f74b8e05" /></p>



## Bitácora de reflexión

**fuerza, aceleración, velocidad y posición**

La posición de un objeto se ve afectada por su velocidad, ya que, la posición va a ir variando según la dirección de la velocidad y que tanto avanza según la magnitud de la velocidad. Ahora, la velocidad está influenciada por la aceleración, ya que si no hay aceleración (acc=0) la velocidad es constante, mientras que si sí tiene un valor, la velocidad va a ir aumentando o disminuyendo en función del valor (pequeño o grande) de la aceleración, es como su tasa de cambio. Luego, cuando vamos a revisar que es la aceleración, esta es la sumatoria de las fuerzas que están afectando a ese objeto, entonces por ello es que cuando antes no jugabamos con las fuerzas, la aceleración de nuestro sistema ficticio quedaba a nuestra decisión, pero ahora con las fuerzas involucradas, todas aportan a la aceleración del objeto.


```js
class guides {
  constructor() {
    this.position = createVector(random(0,windowWidth), random(0,windowHeight));
    this.mass = 10;
    this.G = 1;
    this.azul = color();
    
    this.colorsCalder = [azul,rojo,amarillo];
    this.guideColor = random(colorsCalder);
  }

  show() {
    fill(this.guideColor, 50, 100, 100);
    noStroke();
    circle(this.position.x, this.position.y, 200, 20);
  }

  attrack(m) {
    let force = p5.Vector.sub(this.position, m.position);
    let d = constrain(force.mag(), 5, 25);
    let strength = (this.G * this.mass * m.mass) / (d * d);
    force.setMag(strength);
    return force;
  }
}
```

```js
class Mover {
  constructor() {
    this.mass = 5;
    this.position = createVector(width / 2, 30);
    this.velocity = createVector(0, 0);
    this.acceleration = createVector(0, 0);
    this.arrows = createVector(width / 2, height / 2);
    this.topspeed = 5;
  }

  applyForce(force) {
    let f = p5.Vector.div(force, this.mass);
    this.acceleration.add(f);
  }

  update() {
    this.velocity.add(this.acceleration);
    this.velocity.limit(this.topspeed);
    this.position.add(this.velocity);
    this.acceleration.mult(0);
  }

  show() {
    noStroke();
    strokeWeight(2);
    fill(0);
    ellipse(this.position.x, this.position.y, 2, 2);
  }

  checkEdges() {
    if (this.position.x > width) {
      this.position.x = width;
      this.velocity.x *= -1;
    } else if (this.position.x < 0) {
      this.velocity.x *= -1;
      this.position.x = 0;
    }
    if (this.position.y > height) {
      this.velocity.y *= -1;
      this.position.y = height;
    } else if (this.position.y < 0) {
      this.velocity.y *= -1;
      this.position.y = 0;
    }
  }
}

```

```js
let port;
let connectBtn;
let moverAWSD;
let moverArrows;
let windAWSD;
let windArrows;

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  moverAWSD = new Mover();
  moverArrows = new Mover();
  windAWSD = createVector(0, 0);
  windArrows = createVector(0, 0);
}

function draw() {
  connect();
  windAWSD.set(0, 0);
  windArrows.set(0, 0);
  keys();

  //let gravity = createVector(0, 0.1);
  //moverAWSD.applyForce(gravity);
  //moverArrows.applyForce(gravity);

  moverAWSD.applyForce(windAWSD);
  moverArrows.applyForce(windArrows);
  moverAWSD.update();
  moverAWSD.show();
  moverAWSD.checkEdges();
  moverArrows.update();
  moverArrows.show();
  moverArrows.checkEdges();
}

function keys() {
  if (keyIsDown(LEFT_ARROW)) {
    console.log("LEFT_ARROW");
    windArrows.add(-0.1, 0);
  }
  if (keyIsDown(RIGHT_ARROW)) {
    windArrows.add(0.1, 0);
  }
  if (keyIsDown(UP_ARROW)) {
    console.log("UP_ARROW");
    windArrows.add(0, -0.1);
  }
  if (keyIsDown(DOWN_ARROW)) {
    console.log("DOWN_ARROW");
    windArrows.add(0, 0.1);
  }
  if (keyIsDown(65)) {
    console.log("A");
    windAWSD.add(-0.1, 0);
  }
  if (keyIsDown(68)) {
    console.log("D");
    windAWSD.add(0.1, 0);
  }
  if (keyIsDown(87)) {
    console.log("W");
    windAWSD.add(0, -0.1);
  }
  if (keyIsDown(83)) {
    console.log("S");
    windAWSD.add(0, 0.1);
  }
}

function connect(){
  if (!port.opened()) {
        connectBtn.html('Connect to micro:bit');
    }
    else {
        connectBtn.html('Disconnect');
    }
}

```


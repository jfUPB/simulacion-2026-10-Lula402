# Unidad 3

## Bitácora de proceso de aprendizaje

**ACTIVIDAD 1**
Lo que siento se puede resumir muy bien en dos palabras: abrumada e impotente. 

El mundo cambió muy rápido con la IA y.
Yo le digo a mis amigas, que justo estudian carreras las cuales la IA no se puede apropiar facilmente, que las envidio, que nosotros como artistas, diseñadores y programadores estamos muy golpeados por esto. Les digo que desearía tuvieran empatía y supieran valorar el trabajo de esas personas.

Yo estudio esto porque yo lo quiero hacer con mis manos y mi cerebro, no quiero que una máquina lo haga por mi, yo quiero poder salir a trabajar y saber que lo hice yo y no una granja de computadores. Suena hasta irónico, pero yo quiero esforzarme en hacer las cosas, yo quiero "sufrir", mejorar, incluso sentir compentencia por otros, mas no saber que todos usamos IA y que es cuestión de quien promtee mejor. 

Ya poniendome asi más pesonal, me pone triste saber que, aprendo un poco más lento o me cuesta más aprender algunas cosas, y que si no uso IA me voy a quedar detrás de mis compañeros


**ACTIVIDAD 2**


**ACTIVIDAD 3**

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


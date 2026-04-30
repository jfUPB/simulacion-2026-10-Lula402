# Unidad 7

## Bitácora de proceso de aprendizaje

## ACTIVIDAD 1

| . | Ejemplo 1  | Ejemplo 2 | Ejemplo 3  | Ejemplo 4 |
| ------------- | ------------- | ------------- | ------------- | ------------- |
| **Ji Lee**  |<img width="750" height="750" alt="image" src="https://github.com/user-attachments/assets/4ebac057-bba0-4ea9-8db6-15dac6a07c7c" />| <img width="750" height="750" alt="image" src="https://github.com/user-attachments/assets/caeebd6b-b5d3-4901-ae8c-c9deb6b4b9c0" />| <img width="750" height="750" alt="image" src="https://github.com/user-attachments/assets/a0647454-a7b5-408b-9510-8bde20431889" />|<img width="750" height="750" alt="image" src="https://github.com/user-attachments/assets/4a773bd4-4bd5-4040-9e7d-c06086c6dda5" />|
| **Explicación de cómo la manipulación tipográfica refuerza el significado** | la manera en la que la letra _**X**_ está en pánico, grita y corre hacia la salida (letra _**I**_) refuerza que la palabra Exit guia la evacuación en una emergencia y también que probablemente es solo usada en esos momentos de accidentes | Esa manipulación o mejor dicho refuerzo de esas 5 letras en esas 5 palabras está top, porque refuerza esos simbolos que hemos visto durante toda nuestra vida y los reconocemos ya sin tener que estar acompañados de la palabra. | Asi tal cual es como se percibe el insomnio, de hecho lo refuerza tan fuerte al poner los punticos de la I en blanco, con todo lo otro oscuro, que uno facilmente se puede imaginar un muñequito con los ojos abiertos en la oscuridad. | Probablemente lo primero que se le viene a uno a la mente con Manhatan son las elites, la moda y sus edificios altos. La manipulación de las letras que se pueden alargar es perfecta porque fue como dibujar esa forma tan especial de esa ciudad. |

Las palabras que propongo:

| Peculiar  | Reverse | Water |
| ------------- | ------------- | ------------- |
| <img width="1280" height="958" alt="image" src="https://github.com/user-attachments/assets/bffa5ac4-4117-4f3c-8bf4-58e6a392338a" />| <img width="1600" height="1203" alt="image" src="https://github.com/user-attachments/assets/03a54b5f-7eee-436e-95e6-7781db36d039" />| <img width="1600" height="1204" alt="image" src="https://github.com/user-attachments/assets/42f962e0-3794-4768-bbca-3bd2671db65a" />|

La palabra que más me interesa es peculiar, porque se siente muy viva y cada letra con su personalidad, haciendo que la palabra en general se vea literalmente peculiar y se entienda la intención. Esa palabra siempre me ha gustado en general, porque no me gusta que siempre se busque que las cosas sean "normales", me gusta cada cosa única y con su rareza que lo hace especial.


## ACTIVIDAD 2

1. Explica con tus palabras qué hace cada uno de esos conceptos.

_**Engine:**_ 
El engine es donde actualizamos las físicas de nuestro entorno. Donde antes haciamos el update del motion 101, ahora con matter.js lo llamraremos engine.

_**Composite:**_ 
Los entiendo literalmente como una composición. Es una cajita en la que puedo guardar objetos e incluso objetos de diferentes partes.

_**World:**_ 
Un mundo es básicamente un composite bien completo y complejo, porque es esa cajita, pero contiene todos los objetos de world.

_**Bodies:**_ 
Es un metodo para crear bodies de geometría básica como un cuadrado o un circulo.

_**Constraint:**_ 
Esto determina que tan unidos están los cuerpos y objetos. Con los constrains se puede jugar mucho para hacer que se vea de una manera particular el movimiento de un objeto, ya que basicamente es una restricción en el movimiento.

_**MouseConstraint:**_ 
Se usa para que podamos interactuar con los bodies de la simulación. Tienen los métodos comunes de siempre, como mouseUp, mouseDown, etc, entonces se puede jugar mucho con los conceptos de la simulación y la interación.

2. Replica al menos dos experimentos básicos integrando Matter.js con p5.js.

_**EXPERIMENTO #1**_ 

<p align=center>
<img width="987" height="728" alt="image" src="https://github.com/user-attachments/assets/ddee4041-ca5b-4c6f-9150-152a3f91b562" />
</p>

<p align=center>
<img width="987" height="728" alt="image" src="https://github.com/user-attachments/assets/4faac451-1aac-40da-9716-bd74528ad562" />
</p>


_**Link:**_ https://editor.p5js.org/Lula402/full/bUkDos1rU

```
const { Engine, Bodies, Composite, Events, World } = Matter;

let engine;
let world;
let ball;
let ballColor = "#f5d259";

function setup() {
  createCanvas(800, 600);
  rectMode(CENTER);

  engine = Engine.create();
  world = engine.world;
  let ground = Bodies.rectangle(400, 580, 810, 60, { isStatic: true, friction: 10});

  let sensor = Bodies.rectangle(400, 300, 500, 50, {
    isSensor: true,
    isStatic: true,
  });

  Events.on(engine, "collisionStart", function (event) {
    let pairs = event.pairs;

    for (let i = 0; i < pairs.length; i++) {
      let pair = pairs[i];

      if (
        (pair.bodyA === sensor && pair.bodyB === ball) ||
        (pair.bodyA === ball && pair.bodyB === sensor)
      ) {
        ballColor = "#f55a3c";
      }
    }
  });

  Events.on(engine, "collisionEnd", function (event) {
    let pairs = event.pairs;

    for (let i = 0; i < pairs.length; i++) {
      let pair = pairs[i];

      if (
        (pair.bodyA === sensor && pair.bodyB === ball) ||
        (pair.bodyA === ball && pair.bodyB === sensor)
      ) {
        ballColor = "#f5d259";
      }
    }
  });

  Composite.add(world, [ground, sensor]);

  ball = Bodies.circle(400, 100, 30, {
    restitution: 1.1
  });

  Composite.add(world, ball);
}

function draw() {
  background(20);

  Engine.update(engine);

  fill(255);
  noStroke();
  rect(400, 580, 810, 60);

  noFill();
  stroke(255, 0, 0);
  rect(400, 300, 500, 50);

  let pos = ball.position;
  let angle = ball.angle;

  push();
  translate(pos.x, pos.y);
  rotate(angle);

  fill(ballColor);
  noStroke();
  circle(0, 0, 60);
  pop();
}
´´´


_**EXPERIMENTO #2**_ 

<p align=center>
<img width="988" height="733" alt="image" src="https://github.com/user-attachments/assets/c2496aab-50b1-4b99-bb99-058a2e3d86c0" />
</p>

<p align=center>
<img width="955" height="727" alt="image" src="https://github.com/user-attachments/assets/2f79c676-8200-4cc6-8ee0-e02b4383403d" />
</p>

_**Link:**_ https://editor.p5js.org/Lula402/full/EAB0rMKC6

Letra.js
```
class Letra {
  constructor(x, y, char) {
    this.char = char;
    this.size = random(30, 70);
    this.body = Bodies.rectangle(x, y, this.size * 0.6, this.size * 0.8, {
      restitution: 0.6,
      friction: 0.1
    });

    Composite.add(world, this.body);
  }

  show() {
    let pos = this.body.position;
    let angle = this.body.angle;

    push();
    translate(pos.x, pos.y); 
    rotate(angle);      

    fill(0);
    noStroke();
    textSize(this.size);
    textAlign(CENTER, CENTER); 
    text(this.char, 0, 0);
    pop();
  }
}
```

sketch.js
```
const { Engine, World, Bodies, Composite, Mouse, MouseConstraint } = Matter;

let engine;
let world;
let letras = []; 
let mouseConstraint;

function setup() {
  createCanvas(800, 600);
  let cnv = createCanvas(800, 600); 
  engine = Engine.create();
  world = engine.world;

  let ground = Bodies.rectangle(400, 590, 800, 20, { isStatic: true });
  let ceiling = Bodies.rectangle(400, 10, 800, 20, { isStatic: true });
  let leftWall = Bodies.rectangle(10, 300, 20, 600, { isStatic: true });
  let rightWall = Bodies.rectangle(790, 300, 20, 600, { isStatic: true });

  Composite.add(world, [ground, ceiling, leftWall, rightWall]);

  let canvasMouse = Mouse.create(cnv.elt); 
  
  mouseConstraint = MouseConstraint.create(engine, {
    mouse: canvasMouse,
    constraint: { stiffness: 0.2 }
  });
  
  Composite.add(world, mouseConstraint);
}

function draw() {
  background(255);
  Engine.update(engine);

  for (let l of letras) {
    l.show();
  }
}

function mousePressed() {
  let caracteres = "ABCDEFGHIJKLMNOPQRSTUVWXYZ";
  let listaDeLetras = caracteres.split('');
  let charAleatorio = random(listaDeLetras); 
  let nuevaLetra = new Letra(mouseX, mouseY, charAleatorio);
  letras.push(nuevaLetra);
}
```

**Comportamiento fisico que me interesa para mi palabra**

Quiero que las letras de mi palabra sean catapultables. Quiero que salgan volando de diferentes puntos del canvas, una a la vez. Quiero que tengan gravedad y tambien restitution porque quiero que reboten un poco. Quiero que se unan en orden al caer, como si la letra que ya esta en el canvas tenga un iman que atraiga a la que va cayendo. 

## ACTIVIDAD 3


## Bitácora de aplicación 
https://editor.p5js.org/Lula402/sketches/AIiRP9YeE

## Bitácora de reflexión

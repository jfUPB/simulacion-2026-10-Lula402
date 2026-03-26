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

**CONCEPTO**

Represento el ciclo de vida de colonias de hongos en una placa de Petri, desde su expansión orgánica hasta la esterilización clínica. La idea es mostrar la tensión entre el crecimiento biológico libre y el control humano que decide cuándo algo debe dejar de existir.

Fase 1: Siembra. El usuario decide dónde nacen las esporas mediante clicks o aplica gotas de cloro para bloquear el terreno antes de que las colonias se expandan.
Fase 2: Crecimiento. Las distintas especies se desarrollan a su propio ritmo, adquiriendo las texturas, colores y comportamientos biológicos específicos de cada hongo.
Fase 3: Esterilización por calor. El mouse actúa como el fuego de un mechero de laboratorio, permitiendo quemar las colonias existentes para limpiar y esterilizar la placa de Petri.

**BOCETOS**

![WhatsApp Image 2026-03-26 at 1 58 59 PM](https://github.com/user-attachments/assets/33f72ad1-486d-4c89-96a8-f528a1adc719)


**LINK**

https://editor.p5js.org/Lula402/full/rK2nozpDV

**CÓDIGO**

AspergillusFlavus
```js
class AspergillusFlavus extends Particle {
  show() {
    push();
    translate(this.position.x, this.position.y); 
    let b = random(80, 100); 
    let a = map(this.lifespan, 0, 255, 0, 15);
    fill(45, 80, b, a);
    noStroke();
    circle(0, 0, random(2, 6));
    circle(random(-2, 2), random(-2, 2), random(2, 6));
    circle(random(-2, 2), random(-2, 2), random(2, 6));
    pop();
  }
}
```

Epicoccum
```js
class Epicoccum extends Particle {
  
  constructor(x, y) {
    super(x, y);
    this.velocity.mult(0.5);
    this.color = random(15,35);
  }
  
  show() {
    push();
    translate(this.position.x, this.position.y);
    noStroke();
    fill(this.color, 90,100,8);
    circle(random(-2, 2), random(-2, 2), 10);
    pop();
  }
}

```

FusariumOxysporum
```js
class FusariumOxysporum extends Particle {
  constructor(x, y) {
    super(x, y);
    this.miColor = random(280, 360); 
    this.velocity.mult(0.3);
  }

  update() {
    super.update();
    this.lifespan += 1.5;
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    noStroke();
    
    let tam = map(this.lifespan, 255, 0, 5, 20); 
    
    fill(this.miColor, 80, 40, 5); 

    ellipse(0, 0, tam, tam * 0.8); 
    pop();
  }
}

```

Rhodotorula
```js
class Rhodotorula extends Particle {
  constructor(x, y) {
    super(x, y);
    this.velocity.mult(0.5);
  }

  show() {
    push();
    translate(this.position.x, this.position.y);
    noStroke();
    fill(330, 40, 100, 5);
    circle(0, 0, 15);
    let alphaCuerpo = map(this.lifespan, 0, 255, 0, 20);
    fill(330, 80, 50, alphaCuerpo);
    circle(0, 0, 8);

    fill(0, 0, 100, 40);
    circle(-2, -2, 2);
    pop();
  }
}

```

Zygomycetes
```js
class Zygomycetes extends Particle {
  constructor(x, y) {
    super(x, y); 
    this.velocity.mult(1.5); 
  }
  
  show() {
    push();
    translate(this.position.x, this.position.y); 
    rotate(this.velocity.heading());
    let a = map(this.lifespan, 255, 0, 5, 40); 
    stroke(0, 0, 100, a);
    strokeWeight(2);
    let tam = map(this.lifespan, 255, 0, 5, 10); 
    line(0, 0, tam, 0);
    line(-2, -2, tam, 0);
    line(2, 2, tam, 0);
    pop();
  }
}

```

Emitter
```js
class Emitter {
  constructor(x, y) {
    this.origin = createVector(x, y);
    this.particles = [];
    this.especie = floor(random(6));
    this.estaQuemada = false; 
  }

  addParticle() {
    if (this.especie == 0) {
      this.particles.push(new Penicilina(this.origin.x, this.origin.y));
    } else if (this.especie == 1) {
      this.particles.push(new AspergillusFlavus(this.origin.x, this.origin.y));
    } else if (this.especie == 2) {
      this.particles.push(new FusariumOxysporum(this.origin.x, this.origin.y));
    } else if (this.especie == 3) {
      this.particles.push(new Rhodotorula(this.origin.x, this.origin.y));
    } else if (this.especie == 4) {
      this.particles.push(new Zygomycetes(this.origin.x, this.origin.y));
    } else if (this.especie == 5) {
      this.particles.push(new Epicoccum(this.origin.x, this.origin.y));
    }
  }

  run(gotas, modo) {
    if (modo === 3) {
      let dFuego = dist(mouseX, mouseY, this.origin.x, this.origin.y);
      if (dFuego < 10) {
        this.estaQuemada = true;
      }
    }

    for (let i = this.particles.length - 1; i >= 0; i--) {
      let p = this.particles[i];
      if (this.estaQuemada) p.lifespan = -1;
      p.rebelarseAlCloro(gotas);
      p.run();

      if (p.isDead()) {
        this.particles.splice(i, 1);
      }
    }
  }

  debeDesaparecer() {
    return this.estaQuemada && this.particles.length === 0;
  }
}

```

Particle
```js
class Particle {
  constructor(x, y) {
    this.position = createVector(x, y);
    this.acceleration = createVector(0, 0);
    this.velocity = p5.Vector.random2D();
    this.lifespan = 255.0;
  }

  run() {
    this.update();
    this.show();
  }

  applyForce(force) {
    this.acceleration.add(force);
  }

update() {
  this.rebelarseAlCloro(cloro);
  this.velocity.add(this.acceleration);
  this.velocity.mult(0.98); 
  this.position.add(this.velocity);
  this.lifespan -= 2;
  this.acceleration.mult(0);
  
  let d = dist(this.position.x, this.position.y, width / 2, height / 2);
  
  if (d > petriRadio) {
    this.velocity.mult(0);
    
    let angulo = atan2(this.position.y - height / 2, this.position.x - width / 2);
    this.position.x = width / 2 + cos(angulo) * petriRadio;
    this.position.y = height / 2 + sin(angulo) * petriRadio;
    
    this.lifespan -= 5; 
  }
}

  show() {
    stroke(0, this.lifespan);
    strokeWeight(2);
    fill(127, this.lifespan);
    circle(this.position.x, this.position.y, 8);
  }

  isDead() {
    return this.lifespan < 0.0;
  }
  
  
  rebelarseAlCloro(gotas) {
  for (let g of gotas) {
    let fuerza = p5.Vector.sub(this.position, g);
    let d = fuerza.mag();
    
    if (d < 60) {
      fuerza.setMag(0.1); 
      this.applyForce(fuerza);
    }
  }
}
}

```

Penicilina
```js
class Penicilina extends Particle {
  show() {
    push();
    translate(this.position.x, this.position.y);
    noStroke();

    let colorBorde = color(0, 0, 100, 10);
    let colorCentro = color(170, 70, 100, 10);
    let cantidad = map(this.lifespan, 0, 255, 1, 0);
    let colorFinal = lerpColor(colorCentro, colorBorde, cantidad);
    fill(colorFinal);
    circle(0, 0, random(5, 10));
    pop();
  }
}

```

sketch
```js
//By Lula

let colonias = [];
let petriRadio = 300;
let modo = 1; 
let cloro = [];

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100,100,100);
}

function draw() {
  background(0);
  UI();
  
  for (let c of cloro) {
    noStroke();
    fill(0, 0, 100, 10); 
    circle(c.x, c.y, 25);
  }
  
  for (let i = colonias.length - 1; i >= 0; i--) {
    let col = colonias[i];

    if (!col.estaQuemada) {
      col.addParticle();
    }
    
    col.run(cloro, modo); 

    if (col.estaQuemada && col.particles.length === 0) {
      colonias.splice(i, 1);
    }
  }
  
  dibujarCursor();
  
  fill(255);
  noStroke();
  text("Modo: " + (modo === 1 ? "SIEMBRA" : modo === 2 ? "CLORO" : "FUEGO"), 20, 30);
}

function mousePressed() {
  let d = dist(mouseX, mouseY, width/2, height/2);
  if (d < petriRadio) {
    colonias.push(new Emitter(mouseX, mouseY));
  }
}

/*function UI(){
  fill(175, 50, 50, 50);
  stroke(175, 50, 50, 80);
  strokeWeight(5);
  circle(width/2, height/2, petriRadio * 2);
}*/

function UI() {
  fill(175, 50, 50, 20);
  stroke(175, 50, 50, 40);
  strokeWeight(2);
  circle(width/2, height/2, petriRadio * 2);

  noFill();
  stroke(0, 0, 100, 50); 
  strokeWeight(4);
  circle(width/2, height/2, petriRadio * 2 + 10);
  
  stroke(0, 0, 100, 80);
  strokeWeight(2);
  arc(width/2, height/2, petriRadio * 2 - 10, petriRadio * 2 - 10, PI + QUARTER_PI, TWO_PI - QUARTER_PI);
}

function keyPressed() {
  if (key === '1') modo = 1;
  if (key === '2') modo = 2;
  if (key === '3') modo = 3;
}

function mousePressed() {
  let d = dist(mouseX, mouseY, width/2, height/2);
  if (d < petriRadio) {
    if (modo === 1) {
      colonias.push(new Emitter(mouseX, mouseY));
    } else if (modo === 2) {
      cloro.push(createVector(mouseX, mouseY));
    }
  }
}

function dibujarCursor() {
  push();
  noFill();
  strokeWeight(1);
  if (modo === 1) { 
    stroke(0, 0, 100, 100);
    line(mouseX - 8, mouseY, mouseX + 8, mouseY);
    line(mouseX, mouseY - 8, mouseX, mouseY + 8);
  } else if (modo === 2) { 
    stroke(180, 80, 80, 40);
    circle(mouseX, mouseY, 25);
  } else if (modo === 3) { 
    stroke(20, 100, 100, 80);
    circle(mouseX, mouseY, 40);
    fill(20, 100, 100, 50);
    circle(mouseX, mouseY, 25);
  }
  pop();
}
```

**SS**

FASE 1: SIEMBRA
<img width="434" height="336" alt="image" src="https://github.com/user-attachments/assets/abf7ec5e-a038-4e0f-92d0-3f0fd602aa78" />

FASE 2: CLORO
<img width="415" height="334" alt="image" src="https://github.com/user-attachments/assets/379c6f7c-bce8-42f3-b74c-e82d87256bb9" />


FASE 3: FUEGO
<img width="434" height="336" alt="image" src="https://github.com/user-attachments/assets/5438ffec-6462-460f-8197-732908a24b82" />


## Bitácora de reflexión

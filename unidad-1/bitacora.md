# Unidad 1

## Bitácora de proceso de aprendizaje
### Actividad 1: ¿Qué es la aleatoriedad en mis palabras?
Para mi la aleatoriedad influye en el arte generativo al hacerlo posible. Tenemos en nuestras vidas cotidianas un mundo lleno de imperfecciones, pero dentro de los computadores todo es exacto, calculado y "perfecto", pero entonces ahí entra en juego la aleatoriedad. La aleatoriedad es lo que nos permite a nosotros como humanos "meter mano" a propósito en ese mundo de algoritmos tan "perfecto" y crear outcomes impredecibles. Sin la aleatoriedad el arte generativo no sería posible, nosotros somos demasiado cambiantes y el código es demasiado rígido, mientras que juntos se crea el balance perfecto.

### Actividad 2: experimento en "A traditional random walk"

**_Modificación realizada:_** La modificación que realicé fue que creé otro walker llamado walkerLuisa, a este walker le cambié el color del stroke para identificarlo. También añadí un nuevo comportamiento dependiendo en la función step(), dependiendo de cual sea el objeto.
 
**_Lo que espero que suceda:_** que el walker original pinte en negro y el walkerLuisa pinte en rosado, que al moverse sea con un mirror, ya que comparten la misma función step(), solo que el comportamiento será el contrario.
 
**_Lo que sucedió realmente:_** No sucedió nada, el código continuó como el original.


**_Ocurrió lo que esperaba?:_** no ocurrió, creo que fue porque asigné mal el color del stroke y por eso no se ve. 

Nota: intenté corregirlo y logré que se vea el walker rosado, pero no se mueve en mirror, se mueven muy diferente ambos.

<img width="148" height="121" alt="image" src="https://github.com/user-attachments/assets/631a1b40-e178-43dc-b335-4d2d09144ad3" />


### Actividad 3: Gaussian 

- La diferencia es que en la uniforme todos los datos tienen la misma oportunidad salir, pero en la no uniforme, un dato (la media) es el que tiene más oportunidad de salir sobre los otros, por eso se crea una tendencia hacia el centro.

```js
// The Nature of Code
// Original por: Daniel Shiffman
// http://natureofcode.com
// Modificado por: Luisa GG

let walker;
let colorWalkers;

function setup() {
  createCanvas(640, 240);
  walker = new Walker();
  walkerLuisa = new Walker();
  background(255);
}

function draw() {
  walker.step();
  walker.show(0);
  walkerLuisa.step();
  walkerLuisa.show('#F88ACB');
}

class Walker {
  constructor() {
    this.x = width / 2;
    this.y = height / 2;
  }

  show(colorWalkers) {
    stroke(colorWalkers);
    point(this.x, this.y);
  }

  step() {
    const choice = floor(random(4));
        if (choice == 0) {
          if (this == walker)
              this.x++;
          if (this == walkerLuisa)
              this.x = randomGaussian(540,20);
          
        } else if (choice == 1) {       
           if (this == walker)
              this.x--;
           if (this == walkerLuisa)
              this.x = randomGaussian(540,20);
          
        } else if (choice == 2) {
           if (this == walker)
              this.y--;
           if (this == walkerLuisa)
              this.y = randomGaussian(120,20);
          
        } else {         
          if (this == walker)
              this.y++;         
          if (this == walkerLuisa)
              this.y = randomGaussian(120,20);
        }   
  }
}
```

### Actividad 4: Distribución Normal

**CÓDIGO**
```js
let pizzita;

function setup() {
  createCanvas(400, 400);
  background(255);
  angleMode(DEGREES);
  colorMode(HSB);
  pizzita = new pizza();
}

function draw() {
  pizzita.step();
  pizzita.show();
}

class pizza {
  constructor() {
    this.x1 = 200;
    this.y1 = 200;
    this.radius = 80;
    this.angle = 0;
    this.offset = 10;   
  }

  show() { 
    let alphaLoco = randomGaussian(55, 10);  
    let alphaPizza = constrain(alphaLoco, 40, 70);
    let colorPizza = color(random(0,50), 80, 100, alphaPizza);
    stroke(colorPizza);
    fill(colorPizza);
    triangle(this.x1, this.y1, this.x2, this.y2, this.x3, this.y3)
  }

  step() {
    this.angle = this.angle + randomGaussian(0, 15);
    let a1 = this.angle
    let a2 = this.angle + this.offset
    
    this.x2 = this.x1 + this.radius * cos(a1)
    this.y2 = this.y1 + this.radius * sin(a1)
    this.x3 = this.x1 + this.radius * cos(a2)
    this.y3 = this.y1 + this.radius * sin(a2)
  }
}
```

**LINK**
https://editor.p5js.org/Lula402/full/OGXnLVepe

**CAPTURAS**

<p align = center>
<img width="183" height="185" alt="image" src="https://github.com/user-attachments/assets/683f87b4-2233-420c-953d-8015bba362b6" />
</p>

<p align = center>
<img width="185" height="176" alt="image" src="https://github.com/user-attachments/assets/a90db073-4b7b-47c8-ab65-272ba6caf8ca" />
</p>

### Actividad 5: Lévy flight
**EXPLICACIÓN**
Usé el salto o vuelo de Lévy para ver la diferencia frente a una caminata clásica, una gaussiana y una usando Lévy. Esperaba un movimiento mas concentrado y que de la nada unos cuantos punticos rosados se vieran lejos, pero lo que se ve realmente es locura, los punticos rosados estan totalmente dispersos por el canvas. Para bajarle a la locura puedo disminuir el stepSize para que los saltos no sean tan largos y también podría aumentar el Alpha de la formulita de Lévy, para que asi la probabilidad de que se haga un salto grande disminuya y se vea más concentrado.

**CÓDIGO**
```js
// The Nature of Code
// Daniel Shiffman
// http://natureofcode.com

let walker;
let colorWalkers;

function setup() {
  createCanvas(640, 240);
  walkerOG = new Walker("OG");
  walkerLevy = new Walker("levy");
  walkerGauss = new Walker("gauss");
  background(255);
}

function draw() {
  walkerOG.step();
  walkerOG.show(0);
  walkerLevy.step();
  walkerLevy.show("#F88ACB");
  walkerGauss.step();
  walkerGauss.show("#009688");
}

class Walker {
  constructor(type) {
    this.x = width / 2;
    this.y = height / 2;
    this.type = type;
  }

  show(colorWalkers) {
    stroke(colorWalkers);
    point(this.x, this.y);
  }

  step() {
    if (this.type === "OG") {
      const choice = floor(random(4));
      if (choice == 0) {
        this.x++;
      } else if (choice == 1) {
        this.x--;
      } else if (choice == 2) {
        this.y--;
      } else {
        this.y++;
      }
    }
    if (this.type === "gauss") {
      this.x = randomGaussian(540, 20);
      this.y = randomGaussian(120, 20);
    }

    if (this.type === "levy") {
      let angle = random(TWO_PI);
      let stepSize = levy() * 10;

      let dx = stepSize * cos(angle);
      let dy = stepSize * sin(angle);

      this.x += dx;
      this.y += dy;
      this.x = constrain(this.x, 0, width);
      this.y = constrain(this.y, 0, height);
    }
  }
}

function levy() {
  while (true) {
    let r1 = random(1);
    let probability = pow(r1, 2);
    let r2 = random(1);

    if (r2 < probability) {
      return r1;
    }
  }
}
```

**LINK**

https://editor.p5js.org/Lula402/full/CM-2z1QDY

**CAPTURA**

<p align = center>
<img width="469" height="176" alt="image" src="https://github.com/user-attachments/assets/b6af956f-42fa-4920-bf88-c986bbf070f5" />
</p>

### Actividad 6: Ruido Perlin
**EXPLICACIÓN**
El concepto del ruido perlin lo visualicé como algo respirando, en este caso un circulo, debido a que ese ruido perlin muestra como se puede hacer de lo random algo orgánico. Entonces si se supone que con el Perlin los valores van a ser cercanos al anterior, entonces el cambio en el radio se iba a ver suave. Si quisiera que se mueva más brusco puedo aumentar el tiempo, para que al calcular el noise ya no sea tan delicado el salto entre el valor que elige y el anterior. El resultado obtenido es el esperado.

**CÓDIGO**
```js
// By Lula
let tiempo = 0;

function setup() {
  createCanvas(400, 400);
}

function draw() {
  background(0);
  valorSuave = noise(tiempo)
  radius = map(valorSuave, 0 , 1 , 5 ,200); 
  let col = map(valorSuave, 0, 1, 100, 255);
  fill(col, 150, 255);
  circulo(radius);
  tiempo += 0.02;
}

function circulo(r){
  ellipse(width/2, height/2, r, r)
}
```

**LINK**

https://editor.p5js.org/Lula402/full/Y84Aspkrq

**CAPTURA**

<p align = center>
<img width="199" height="209" alt="image" src="https://github.com/user-attachments/assets/cfe43e6e-b1c8-4d22-9fa1-e013cdb16d84" />
</p>

<p align = center>
<img width="198" height="193" alt="image" src="https://github.com/user-attachments/assets/fb9b31a2-beb5-4917-9427-823793112ad5" />
</p>



## Bitácora de aplicación 

**EXPLICACIÓN**

Frente a la situación actual del mundo, especificamente la del hemisferio norte, muchas personas han hecho la comparación con el año 1939, el año que empezó la segunda guerra mundial. En EE.UU las situaciones y desiciones que se han tomado ultimamente, se asemejan mucho a las de la época de 1939. Para plasmar esto, quise que estados unidos (los punticos de colores) se vieran encerrados por los "1939", esto representa que estan encerrados en el pasado. Cuando el usuario presiona > viaja al futuro (2026), donde ya EE.UU se libera, explota y palpita, los años 1939 dejan de encerrarlo, esto simboliza una revolución. El usuario con las flechas < > "viaja en el tiempo" y cuando esta en el pasado al mover el mouse cambia el color del año 1939, pintando asi la bandera de alemania.

**CÓDIGO**

```js
let lienzoPunticos;
let lienzoExplosion;
let lienzoAños;
let BG;
let azul;
let rojo;
let blanco;
let colorPunticos;
let año;

function preload() {
  BG = loadImage("map.png");
}

function setup() {
  colorMode(HSB);
  createCanvas(898, 410);
  lienzoPunticos = createGraphics(898, 410);
  lienzoExplosion = createGraphics(898, 410);
  lienzoAños = createGraphics(898, 410);
  lienzoPunticos.clear();
  lienzoAños.clear();
  BG.filter(INVERT)
  azul = new Puntico("gauss");
  rojo = new Puntico("gauss");
  blanco = new Puntico("gauss");
  año = new Año(azul);
}

function draw() {
  background(0);
  image(BG, 0, 0);

  let valorSuave = noise(frameCount * 0.08);
  let escala = map(valorSuave, 0, 1, 0.98, 1.02);

  if (azul.modo == "perlin") {
    push();
    translate(width / 2, height / 2);
    scale(escala);
    image(lienzoPunticos, -width / 2, -height / 2);
    image(lienzoExplosion, -width / 2, -height / 2);
    pop();
  } else {
    image(lienzoPunticos, 0, 0);
    image(lienzoAños, 0, 0);
  }

  azul.step();
  azul.show("#3F51B5");
  rojo.step();
  rojo.show("#DC0F00");
  blanco.step();
  blanco.show("#FFFFFF");

  if (azul.modo == "gauss" && frameCount % 30 == 0) {
    año.step();
    año.show();
  }
}
  

class Puntico {
  constructor(modo) {
    this.x = width / 2;
    this.y = height / 2;
    this.modo = modo;
    this.tiempo = random(1000);
    this.radius = 1;
  }

show(colorPunticos) {
  if (this.modo == "gauss") {
    lienzoPunticos.stroke(colorPunticos);
    lienzoPunticos.strokeWeight(2);
    lienzoPunticos.point(this.x, this.y);
  } else {
    lienzoExplosion.stroke(colorPunticos);
    lienzoExplosion.strokeWeight(2);
    lienzoExplosion.point(this.x, this.y);
  }
}

  step() {
    if (this.modo == "gauss") {
      this.radius = 1;
      this.x = randomGaussian(110, 15);
      this.y = randomGaussian(140, 15);
      let pos = {x: this.x, y: this.y};
    } else if (this.modo == "perlin") {
      this.x = random(width);
      this.y = random(height);
    }
  }
}

class Año {
  constructor(puntoReferencia) {
    this.x = width / 2;
    this.y = height / 2;
    this.refe = puntoReferencia;
  }

show() {
  let colores = ["#FFFFFF", "#FFC107", "#DC0F00"];
  let indice = floor(map(mouseX, 0, width, 0, colores.length));
  indice = constrain(indice, 0, colores.length - 1);
  let colorElegido = colores[indice];
  
  lienzoAños.fill(colorElegido);
  lienzoAños.noStroke(0);
  lienzoAños.textSize(8);
  lienzoAños.text("1939", this.x, this.y); 
}

  step() {
    this.speed = 5;
    let salto = pow(levy(), 0.3);
    let angle = random(TWO_PI);
    let radioMinimo = 70;
    let radioMaximo = 75;
    let stepSize = radioMinimo + salto * this.speed;
    stepSize = constrain(stepSize, radioMinimo, radioMaximo);

    let dx = stepSize * cos(angle);
    let dy = stepSize * sin(angle);

    this.x = this.refe.x + dx;
    this.y = this.refe.y + dy;
    this.x = constrain(this.x, 0, width);
    this.y = constrain(this.y, 0, height);
  }
}

function levy() {
  while (true) {
    let r1 = random(1);
    let probability = pow(r1, 2);
    let r2 = random(1);

    if (r2 < probability) {
      return r1;
    }
  }
}

function keyPressed() {
  if (keyCode === RIGHT_ARROW) {
    lienzoAños.clear();
    azul.modo = "perlin";
    rojo.modo = "perlin";
    blanco.modo = "perlin";
  } else if (keyCode === LEFT_ARROW) {
    lienzoExplosion.clear();
    azul.modo = "gauss";
    rojo.modo = "gauss";
    blanco.modo = "gauss";
  }
}
```

**LINK**

https://editor.p5js.org/Lula402/full/l5qT9mCEu

**CAPTURA**

<p align = center>
<img width="602" height="274" alt="image" src="https://github.com/user-attachments/assets/f939a3a1-914b-43fd-ba40-00272cae7684" />
</p>

<p align = center>
<img width="602" height="274" alt="image" src="https://github.com/user-attachments/assets/06586e6f-75a5-4d40-9979-a428c07a5559" />
</p>

## Bitácora de reflexión

**1.**

La diferencia fundamental entre _random()_ y el Ruido Perlin (noise()) es la manera en la que se configura la aleatoriedad. _random()_ es literalmente aleatorio full, todos los números tienen la posibilidad de salir. En noise() la aletoriedad es "delicada", porque noise siempre devuelve un valor entre 0 y 1, el cual va aumentando gradualmente. La perillita que se le pone: noise(perillita) es la que determina que tan brusco rápido se recorren esos valores de 0 a 1. Además noise() tiene continuidad, es decir, si la llamo dos veces seguidas sus valores van a estar relacionados.

**2.**

Una distribución de probabilidad es que posibilidad permito que los valores tengan de existir/salir.

La diferencia visual entre una caminata con distribución uniforme vs una con una distribución normal, es que la uniforme es un caos parejo, porque como todos los valores tienen la misma posibilidad de salir, la caminata es entonces desordenada, pero en su desorden se va rellenando porque todos los valores pueden ir saliendo. La que tiene distribución normal se ve más simetrica y coordinada, ya que uno de los valores tiene más posibilidad de salir sobre los otros, entonces se ve más rellena en ese valor que en los demás.

**3.**

El papel de la aleatoriedad en el arte generativo es primordial, ya que esta es la que permite todos los diferentes outcomes que hacen de cada obra única. Diría que otro de los papeles es que la aleatoriedad pone ese granito de inesperado o de "Imperfección" a un arte hecho con código, que se esperaría fuese "perfecto", es decir, le da ese toque humano.

**4.**

Uno de los que usé fue Lévy Flight, este fue perfecto para los años "1939" que aparecían, ya que justo el efecto que quería era que parecieran "saltando" por ahí y rodearan EE.UU. Lévy Flight permite este efecto al ser una aleatoriedad que en mi caso configuré para favorecer los valores grandes y castigar a los menores. Cuando un valor grande lograba pasar entonces ahí se daba el salto de un 1939.

**5.**










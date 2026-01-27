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


### Actividad 6: Ruido Perlin


## Bitácora de aplicación 



## Bitácora de reflexión






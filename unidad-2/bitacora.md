# Unidad 2

## Bitácora de proceso de aprendizaje

### ACTIVIDAD 1
El trabajo que más me gustó fue el de Zach Lieberman, se llama **Circles, blobs, ripples**. Fue mi favorita porque me parece que logró algo muy lindo que salió desde algo muy simple, un circulo. El dice que analizó como todos salen de un circulo, pero ver como algo cambia en ellos y se transforman es lo especial. Muestra varias imagenes de la exibición y es evidente que todas vienen de un mismo algoritmo, pero se ven totalmente diferentes los resultados, eso me gusta.

<img width="541" height="201" alt="image" src="https://github.com/user-attachments/assets/e50e528a-9c25-456b-84a4-25604cb36d18" />


### ACTIVIDAD 2

**1. ¿Cómo funciona la suma dos vectores en p5.js?**

La suma de vectores en p5.js funciona con el método .add, este se encarga de sumar componente con componente. Esto suma X con X, Y con Y, asi sale un nuevo vector (X,Y).

**2. ¿Por qué esta línea position = position + velocity; no funciona?**

Porque position y velocity no son números, son objetos tipo vector, entonces en js el uso de el operador + no funciona, se debe usar .add para que se sumen los vectores correctamente. 

### ACTIVIDAD 3

**1. ¿Qué tuviste que hacer para hacer la conversión propuesta?**

Para hacer la conversión, todo lo que estuviera calculando manualmente x,y tuve que pasarlo a vectores. Entonces:

- Lo principal fue que en el constructor ahora todo se crea en base a un vector: this.pos = createVector(width / 2, height / 2);
- Dentro de show(); uso las componentes (x,y) del vector creado para dibujar los puntos: point(this.pos.x, this.pos.y);
- Dentro del tipo "OG", lo único fue crear otro vector para representar el step: let step = createVector(0, 0); luego este dependiendo del choice al final se usa .add para poder que se note la modificación en el canvas.
- Para el tipo Gausssian, creé un newX y un newY, son variables en las que se almacena el valor del randomGaussian. Luego este valor va a determinar el (x,y) del vector pos, usando set.
- El tipo Levý fue el más complicado. Se creó un vector unitario basándose en el ángulo: let step = p5.Vector.fromAngle(angle); Luego multiplicamos este unitario por el stepSize, para poder que sea de un tamaño representativo: step.mult(stepSize); Por último añado ese step listo a pos: this.pos.add(step); 


**2. Escribe el código que utilizaste para resolver el ejercicio.**

```js
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
    this.pos = createVector(width / 2, height / 2);
    this.type = type;
  }

  show(colorWalkers) {
    stroke(colorWalkers);
    point(this.pos.x, this.pos.y);
  }

  step() {
    if (this.type === "OG") {
      let step = createVector(0, 0);
      const choice = floor(random(4));

      if (choice == 0) step.x = 1;
      else if (choice == 1) step.x = -1;
      else if (choice == 2) step.y = -1;
      else step.y = 1;
      this.pos.add(step);
    }
    if (this.type === "gauss") {
      let newX = randomGaussian(540, 20);
      let newY = randomGaussian(120, 20);
      this.pos.set(newX, newY);
    }

    if (this.type === "levy") {
      let angle = random(TWO_PI);
      let stepSize = levy() * 10;
      let step = p5.Vector.fromAngle(angle);
      step.mult(stepSize);
      this.pos.add(step); 
      this.pos.x = constrain(this.pos.x, 0, width);
      this.pos.y = constrain(this.pos.y, 0, height);
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

### ACTIVIDAD 4

**1. ¿Qué resultado esperas obtener en el programa anterior?**

Espero que primero imprima la posición inicial del vector (6, 9), luego ejecute una función que cambie esos valores y finalmente imprima el resultado para ver si el vector original cambió o se mantuvo igual.

**2. ¿Qué resultado obtuviste?**

Obtuve este mensaje en consola, lo que significa que el resultado esperado si es el obtenido:

```
p5.Vector Object : [6, 9, 0] 
p5.Vector Object : [20, 30, 0] 
Only once 
```

**3. Recuerda los conceptos de paso por valor y paso por referencia en programación.**

**Paso por valor:** Es cuando le paso a una función una copia del objeto. Si cambio la copia, el original sigue igual.

**Paso por referencia:** Es cuando le paso a la función la dirección de memoria del objeto, entonces lo modifico.

**4. ¿Qué tipo de paso se está realizando en el código?**

Se está relizando paso por referencia, porque estoy afectando al vector original en todo el programa.

**5. ¿Qué aprendiste?**

Refresqué lo que es paso por valor y por referencia. Aprendí que los vectores en p5.js tienen .copy() para poder hacer lo que se hizo pero sin destruir el original.

### ACTIVIDAD 5

**1.** Ambos calculan qué tan largo es el vector (su magnitud). mag() usa la fórmula completa, mientras que magSq() da el resultado sin sacar la raíz cuadrada. magSq() es la más eficiente porque se ahorra el proceso de calcular raíces cuadradas. 


**2.**  Sirve para reducir el tamaño del vector a exactamente 1, como un vector unitario, porque manteniene su dirección. Se usa cuando lo único que importa es saber hacia dónde apunta.

**3.** Es una forma de medir qué tanto se alinean dos vectores: si apuntan en la misma dirección, el resultado es positivo; si son perpendiculares, es cero; y si van en direcciones opuestas, es negativo.

**4.**  El de instancia se llama desde el objeto v1.dot(). El propio vector se compara con otro y generalmete almacenamos este resultado que es un número en otra variable.

El estático se llama desde la librería (p5.Vector.dot(v1, v2)). Ambos vectores se comparan entre si y generalmete almacenamos este resultado que es un número en otra variable.

**5.** geométricamente el producto cruz genera un tercer vector.
Orientación: El nuevo vector es perpendicular (forma un ángulo de 90°) al plano que forman los otros dos. Si los vectores están en el suelo, el producto cruz apunta hacia el techo.
Magnitud: El tamaño de este nuevo vector es igual al área de plano que forman los dos vectores originales. Entre más abiertos estén los vectores, más grande es el área.

**6.**  Sirve para calcular la distancia entre dos puntos (vectores). Se puede usar para saber si dos objetos están chocando o qué tan lejos están.

**7.** normalize(): Convierte cualquier vector en un "vector unitario". Sirve para saber la dirección sin tener que lidiar con la magnitud.

limit(): Establece un tope máximo a la magnitud de un vector que queramos que no crezca mas de cierto número.

### ACTIVIDAD 6

**1.**

```js
function setup() {
    createCanvas(400, 400);
}

function draw() {
    background(200);

    let v0 = createVector(50, 50);
    let v1 = createVector(200, 0);
    let v2 = createVector(0, 200);
    let v3 = p5.Vector.sub(v2, v1);
    let v3_origin = p5.Vector.add(v0, v1);
    let oscilacion = sin(frameCount * 0.02); 
    let t = map(oscilacion, -1, 1, 0, 1);
    let v4 = p5.Vector.lerp(v1, v2, t);
    let rojo = color(255, 0, 0);
    let azul = color(0, 0, 255);
    let colorsito = lerpColor(rojo, azul, t);
    drawArrow(v0, v1, 'red');
    drawArrow(v0, v2, 'blue');
    drawArrow(v0, v4, colorsito);
    drawArrow(v3_origin, v3, 'green');
}

function drawArrow(base, vec, myColor) {
    push();
    stroke(myColor);
    strokeWeight(3);
    fill(myColor);
    translate(base.x, base.y);
    line(0, 0, vec.x, vec.y);
    rotate(vec.heading());
    let arrowSize = 7;
    translate(vec.mag() - arrowSize, 0);
    triangle(0, arrowSize / 2, 0, -arrowSize / 2, arrowSize, 0);
    pop();
}
```

**2.**

**Lerp()** es una forma de encontrar un punto intermedio entre dos extremos que son. En esta actividad se usó para que la flechita se moviera smoothmente entre la roja y la azul, por eso los valores del lerp eran los vectores principales y el **amt** era el tiempo: t ---> _**p5.Vector.lerp(v1, v2, t);**_

**lerpColor()** funciona exactamente igual pero con colores, te da una mezcla entre dos colores y dependiendo del **amt** se determina que tan cerca esta de el color 1 o del color 2. En esta actividad se usó para el color que cambia en la flechita que se mueve, porque la idea es que pase de rojo -> morado -> azul. El **amt** también fue t, entonces si t es chiqui el color está mas cerca de ser rojo, si está en el medio es morado y entre mas grande, está mas cerca de ser azul.

**3.**

drawArrow() le hace translate al lienzo para que el origen (0,0) esté donde la flecha va a empezar. Luego, dibuja la línea desde (0,0) hasta lo que mida el vector, que en este caso lo cambié a 200. Después usa rotate y vec.heading(), los cuales rotan el canvas y heading() que le dice cuántos grados debe girarlo para que la "punta" mire exactamente hacia donde el vector está señalando.

### ACTIVIDAD 7

1. El motion 101 simula movimiento, es como la posición y la velocidad son influenciadas por la aceleración. Geométricamente esto se ve como una particula se desvía en cierta dirección y con cierta velocidad, esta dirección va cambiando y la velocidad aumentando con el tiempo.

**<p align=center>La Aceleración cambia la Velocidad, y la Velocidad cambia la Posición.</p>**

**Posición:** un vector que va desde el origen (0,0) hasta donde está el objeto.

**Velocidad:** un vector que se suma a la posición en cada frame. Es el paso que da el objeto o mejor dicho, cuanto avanza.

**Aceleración:** un vector que se suma a la velocidad. Es lo que hace que el vector de velocidad cambie y aumente la rapidez o cambie de dirección.

2. 

En este ejemplo se aplica Motion 101 en el método update() que está en la clase Mover (la bolita). Se utiliza .add() para sumar el vector velocidad al vector posición: this.position.add(this.velocity); Esto hace que en cada frame la bolita de desplaze cuanto el vector de velocidad indique. 

En el ejemplo no se está aplicando ninguna aceleración, por lo que la velocidad es constante y por eso la bolita se mueve siempre al mismo ritmo. La posición si va acumulando las anteriores y va cambiando, por lo que se ve que la bolita avanza, porque cada que se llama update() se le suma la velocidad para hallar su nueva posición y dirección.


### ACTIVIDAD 8

**bolita0.aceleracionConstante();** esta se mueve a una velocidad constante y va avanzando diagonalmente. Aparece por arriba y va bajando empujandose un poquito hacia la derecha suavemente.

**bolita1.aceleracionAleatoria();** observo que parece una mosquita, porque se parece mucho a la forma en la que ellas se mueven. Esta bolita tiene un temblorsito y va caminando por todo el canvas sin ningun orden.

**bolita2.aceleracionMouse();** esta me gusta mucho porque es como si tuviera una pelotica con un nilon que estira en mi mano. Cuando muevo el mouse la bolita amarilla me intenta seguir y si lo dejo quieto se queda a mi alrededor con un movimiento parecido al de un péndulo.

```js
let bolita0;
let bolita1;
let bolita2;

function setup() {
  createCanvas(600, 600);
  bolita0 = new Bolita('rgb(180,180,249)');
  bolita1 = new Bolita('#FF8FB5');
  bolita2 = new Bolita('#FFE38F');
}

function draw() {
  background(0);
  
  bolita0.aceleracionConstante();
  bolita1.aceleracionAleatoria();
  bolita2.aceleracionMouse();

  bolita0.update();
  bolita0.checkEdges();
  bolita0.show();
  bolita1.update();
  bolita1.checkEdges();
  bolita1.show();
  bolita2.update();
  bolita2.checkEdges();
  bolita2.show();
}

class Bolita {
  constructor(c) {
    this.pos = createVector(width / 2, height / 2);
    this.vel = createVector(0, 0);
    this.acc = createVector(0, 0);
    this.topSpeed = 5; 
    this.colorsito = c
  }

  update() {
    this.vel.add(this.acc);   
    this.vel.limit(this.topSpeed); 
    this.pos.add(this.vel);   
    this.acc.mult(0);         
  }

  aceleracionConstante() {
    this.acc = createVector(0.01, 0.05); 
  }

  aceleracionAleatoria() {
    this.acc = p5.Vector.random2D().mult(0.2);
  }

  aceleracionMouse() {
    let mouse = createVector(mouseX, mouseY);
    let direccion = p5.Vector.sub(mouse, this.pos);
    direccion.setMag(0.2); 
    this.acc = direccion;
  }

  show() {
    fill(this.colorsito);
    circle(this.pos.x, this.pos.y, 30);
  }

  checkEdges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }
}
```



## Bitácora de aplicación 

**1. CONCEPTO**

Esta obra generativa es en honor a mi grupo de amigas. Somos 6, y cada una de nosotras es muy diferente, pero nos complementamos y hacemos que el grupo sea tan agradable como es. A cada una de las integrantes le corresponde un color que cada una escogió porque se siente representada. Los colores del grupo cambian a sus colores complementarios respectivamente cada que se encuentre con otro grupo de otra amiga, además el tamaño tambien aumenta al chocarse con los grupos. Cada grupo de partículas (cada persona) tiene un comportamiento único, el cual incluye una velocidad, aceleración e interacción específica. A modo de interactividad por medio del teclado con los numeros del 1 al 6, se puede escoger que "personaje" eres, y ese grupo de particulas es atraido o persigue al mouse.


**2. CÓDIGO**

```js
let particulas = [];
let nombresAmigas = ["Mari", "Vale", "Isa", "Lala", "Saris", "Lula"];
let quienSoy = "Mari";

function setup() {
  createCanvas(windowWidth, windowHeight);
  colorMode(HSB, 360, 100, 100, 100);
  for (let nombre of nombresAmigas) {
    for (let i = 0; i < 25; i++) {
      particulas.push(new Bolita(nombre));
    }
  }
}

function draw() {
  background(0);
  for (let b of particulas) {
    b.update(particulas);
    b.display();
  }
}

class Bolita {
  constructor(nombre) {
    this.nombre = nombre;
    this.pos = createVector(random(width), random(height));
    this.vel = createVector(random(-1, 1), random(-1, 1));
    this.acc = createVector(0, 0);

    let perfil = this.obtenerHSB(nombre);

    this.h = perfil.h;
    this.s = perfil.s;
    this.b = perfil.b;

    this.colorBase = color(this.h, this.s, this.b);

    let tonoOpuesto = (this.h + 180) % 360;
    this.colorComplementario = color(tonoOpuesto, this.s, this.b);

    this.colorActual = this.colorBase;
    this.distanciaAlMasCercano = 150;
  }

  obtenerHSB(n) {
    if (n === "Mari") return { h: 5, s: 92, b: 62 };
    if (n === "Vale") return { h: 191, s: 100, b: 78 };
    if (n === "Saris") return { h: 190, s: 69, b: 71 };
    if (n === "Isa") return { h: 283, s: 26, b: 82 };
    if (n === "Lala") return { h: 282, s: 87, b: 62 };
    if (n === "Lula") return { h: 210, s: 24, b: 96 };
    return { h: 0, s: 0, b: 100 };
  }
  update(lista) {
    if (this.nombre === quienSoy) {
      this.atraerAlMouse();
      this.detectarOtroSoloParaColor(lista);
    } else {
      this.comportamientoGrupal(lista);
      this.comportamiento();
    }
    let cantidadMezcla = map(this.distanciaAlMasCercano, 0, 20, 1, 0, true);
    let colorObjetivo = lerpColor(
      this.colorBase,
      this.colorComplementario,
      cantidadMezcla
    );
    this.colorActual = lerpColor(this.colorActual, colorObjetivo, 0.1);
    this.vel.add(this.acc);
    this.pos.add(this.vel);
    this.acc.mult(0);
    this.vel.mult(0.98);
    this.checkEdges();
  }

  atraerAlMouse() {
    let mouse = createVector(mouseX, mouseY);
    let fuerza = p5.Vector.sub(mouse, this.pos);
    fuerza.setMag(0.5);
    this.acc.add(fuerza);
  }

  comportamientoGrupal(todasLasBolitas) {
    let promedio = createVector(0, 0);
    let contador = 0;
    let distanciaDeseada = 10;
    this.distanciaAlMasCercano = 100;
    for (let otra of todasLasBolitas) {
      let d = dist(this.pos.x, this.pos.y, otra.pos.x, otra.pos.y);
      if (otra.nombre === this.nombre && otra !== this) {
        if (d > 0 && d < 100) {
          if (d < distanciaDeseada) {
            let huye = p5.Vector.sub(this.pos, otra.pos);
            huye.setMag(0.5);
            this.acc.add(huye);
          } else {
            promedio.add(otra.pos);
            contador++;
          }
        }
      } else if (otra.nombre !== this.nombre) {
        if (d < this.distanciaAlMasCercano) {
          this.distanciaAlMasCercano = d;
        }
      }
    }

    if (contador > 0) {
      promedio.div(contador);
      let fuerza = p5.Vector.sub(promedio, this.pos);
      fuerza.setMag(0.5);
      this.acc.add(fuerza);
    }
  }

  comportamiento() {
    switch (this.nombre) {
      case "Saris":
        let saris = p5.Vector.random2D();
        saris.mult(0.2);
        this.acc.add(saris);
        break;
      case "Isa":
        let mouse = createVector(mouseX, mouseY);
        let d = dist(this.pos.x, this.pos.y, mouse.x, mouse.y);
        if (d < 200) {
          let fuerza = p5.Vector.sub(mouse, this.pos);
          fuerza.setMag(0.2);
          this.acc.sub(fuerza);
        }
        break;
      case "Mari":
        let ciclo = frameCount % 200;
        if (ciclo < 150) {
          let suave = p5.Vector.random2D();
          suave.mult(0.1);
          this.acc.add(suave);
        } else {
          this.vel.mult(0.9);
        }
        break;
      case "Lala":
        let lalas = p5.Vector.random2D();
        lalas.mult(0.1);
        this.acc.add(lalas);
        break;
      case "Vale":
        let fuerzaVale = this.vel.copy();
        fuerzaVale.setMag(0.08);
        this.acc.add(fuerzaVale);
        break;
      case "Lula":
        let pasoNormal = p5.Vector.random2D();
        pasoNormal.mult(0.1);
        this.acc.add(pasoNormal);

        if (random(1) < 0.001) {
          let salto = p5.Vector.random2D();
          salto.mult(5);
          this.acc.add(salto);
        }
        break;
      default:
        atraerAlMouse();
    }
  }

  detectarOtroSoloParaColor(lista) {
    this.distanciaAlMasCercano = 100;
    for (let otra of lista) {
      if (otra.nombre !== this.nombre) {
        let d = dist(this.pos.x, this.pos.y, otra.pos.x, otra.pos.y);
        if (d < this.distanciaAlMasCercano) {
          this.distanciaAlMasCercano = d;
        }
      }
    }
  }

  display() {
    noStroke();
    fill(this.colorActual);
    let tamaño = map(this.distanciaAlMasCercano, 0, 30, 8, 4, true);
    ellipse(this.pos.x, this.pos.y, tamaño, tamaño);
  }

  checkEdges() {
    if (this.pos.x > width) this.pos.x = 0;
    if (this.pos.x < 0) this.pos.x = width;
    if (this.pos.y > height) this.pos.y = 0;
    if (this.pos.y < 0) this.pos.y = height;
  }
}

function keyPressed() {
  if (key === "1") quienSoy = "Saris";
  if (key === "2") quienSoy = "Isa";
  if (key === "3") quienSoy = "Mari";
  if (key === "4") quienSoy = "Lala";
  if (key === "5") quienSoy = "Vale";
  if (key === "6") quienSoy = "Lula";
}

```

**3. LINK**

https://editor.p5js.org/Lula402/full/_YaAencOa

**4. SS**

<p align= center>
<img width="124" height="100" alt="image" src="https://github.com/user-attachments/assets/77c008c5-3171-4fa9-a27a-dca7b0ee88a1" />
</p>

<p align= center>
<img width="130" height="121" alt="image" src="https://github.com/user-attachments/assets/e677c02a-c4de-4ff8-8ceb-3cab1dab93a7" />
</p>

## Bitácora de reflexión












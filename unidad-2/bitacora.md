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

**3.**

### ACTIVIDAD 7

1. El motion 101 es como la posición y la velocidad son influenciadas por la aceleración. Geométricamente esto se ve como una particula se desvía en cierta dirección y con cierta velocidad, esta dirección va cambiando y la velocidad aumentando con el tiempo.
2. 

### ACTIVIDAD 8

## Bitácora de aplicación 



## Bitácora de reflexión








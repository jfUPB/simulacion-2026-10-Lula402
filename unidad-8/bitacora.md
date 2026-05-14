# Unidad 8

## Bitácora de proceso de aprendizaje


## Bitácora de aplicación 

## 1. Herramienta elegida con justificación
Mis herramientas elegidas fueron Three.js y Strudel.js . Escogí estas dos herramientas porque desde que el profe nos mostró Strudel me encarreté con todo ese mundo de hacer música o ritmos con código. Por otro lado, el motivo de Three fue más por poder hacer un proyecto 3D y poderlo usar en mi compu, ya que Touch o archivos pesados de Blender no los corre bien.

## 2. Sistema transferido
Usé dos sistemas estudiados en el curso: 
Oscillation: Three.
Randomness: Strudel.

## 3. Contexto profesional concreto

Con este final buscaba empezar a diseñar mi portafolio y justo quiero algo así para la Hero section, algo interactivo, creativo que desde el inicio genere curiosidad. Quiero tener pronto mi propio dominio con mi páginita web del portafolio y la idea es ponerle luego los colores que elija para mi “marca” profesional.

## 4. Concepto visual
He con el tiempo intentando buscar mi propio estilo, entre las cosas que más me genera agrado visualmente son los gradients y en específico los “trippy gradients” que son figuras con gradients que tienen un noise y los hace sentir como un mapa de calor. 

## 6. Explicación de transferencia

Oscillation: 

El principio de oscilación de The Nature of Code, donde la posición de cada punto de la esfera se calcula con la función armónica simple de y = amplitud * sin(ángulo). Al trasladar esto al espacio 3D de Three, no se mueven los puntos hacia arriba o hacia abajo como en la serpiente de esa uidad, sino que los desplazo hacia afuera siguiendo su vector normal, lo que hace que la esfera se infle y contraiga como un organismo vivo. Esta oscilación es dinámica, ya que el angulo tiene una velocidad angular que cambia respect al UTIME, la idea era tambien que reaccionara a audio, pero no lo logré. Al tener un angleX y un angleY para la oscilación, las ondas chocan entre sí creando picos y valles que no son uniformes, lo que le da ese look orgánico.

Randomness:

La función rand por defecto en Strudel es uniforme: todos los números tienen la misma probabilidad de salir,asi que con const gauss = rand.add(rand).add(rand).div(3);, refresqué el Teorema del Límite Central:

Sumo tres fuentes de aleatoriedad.

Las promedio dividiendo por 3.

Los valores dejan de ser planos y se agrupan en una distribución Gaussiana. La mayoría de los resultados estarán cerca (0.5), y los extremos (0 o 1) serán raros. 

En lugar de que cada golpe del high hat suene al mismo volumen, el uso de ese gauss hace que la mayoría de los golpes tengan un volumen intermedio. Esto imita a un baterista real, porque la fuerza en la mano varía ligeramente pero se mantiene consistente en un promedio. 

## 7.Pieza final resuelta

Main.js

```js
import * as THREE from 'three';
import { initStrudel, evaluate, getAudioContext, samples } from '@strudel/web';
import { WaveSphere } from './scenes/WaveSphere.js';

// Setup de Three.js 
const renderer = new THREE.WebGLRenderer({ antialias: true });
renderer.setSize(window.innerWidth, window.innerHeight);
renderer.setPixelRatio(Math.min(window.devicePixelRatio, 2));
renderer.setClearColor(0x000000);
document.querySelector('#app').appendChild(renderer.domElement);

const camera = new THREE.PerspectiveCamera(75, window.innerWidth / window.innerHeight, 0.1, 1000);
camera.position.z = 4;

const mouse = { nx: 0, ny: 0 };
window.addEventListener('mousemove', e => {
  mouse.nx = (e.clientX / window.innerWidth)  * 2 - 1;
  mouse.ny = (e.clientY / window.innerHeight) * -2 + 1;
});

// ── Audio Engine ──────────────────────────────────────────────────────────────
let audioStarted = false;
let analyserNode = null;
let analyserBuf  = null;

async function startAudio() {
  if (audioStarted) return;
  audioStarted = true;

  const repl = await initStrudel();
  const ctx = getAudioContext();

  await samples({ 'track': 'female_vocal.mp3' });

  const worklet = repl?.scheduler?.worklet ?? repl?.worklet ?? repl?.audio?.worklet;

  if (worklet) {
    analyserNode = ctx.createAnalyser();
    analyserNode.fftSize = 512;
    analyserNode.smoothingTimeConstant = 1;
    analyserBuf = new Uint8Array(analyserNode.frequencyBinCount);

    worklet.connect(analyserNode);
    analyserNode.connect(ctx.destination);
    console.log("Escuchando frecuencias altas");
  }

  evaluate(`s("track").loopAt(2).gain(0.8)`);
}

function getEnergy() {
  if (!analyserNode) return 0;
  analyserNode.getByteFrequencyData(analyserBuf);
  
  let sum = 0;
  const startBin = Math.floor(analyserBuf.length * 0.5); 
  const endBin = analyserBuf.length;
  const count = endBin - startBin;

  for (let i = startBin; i < endBin; i++) {
    sum += analyserBuf[i];
  }
  
  let energy = (sum / (count * 255)) * 6.0;
  
  return Math.min(energy, 1.0);
}

window.addEventListener('click', startAudio, { once: true });

// ── Animation Loop ────────────────────────────────────────────────────────────
const clock = new THREE.Clock();
const waveScene = new WaveSphere(renderer, camera);

function animate() {
  requestAnimationFrame(animate);
  const energy = getEnergy();
  
  // Mandamos la energía capturada de los agudos
  waveScene.setEnergy(energy);
  
  waveScene.update(clock.getElapsedTime(), mouse);
  waveScene.render();
}

animate();
``` 


WaveSphere.js
``` 
import * as THREE from 'three';

export class WaveSphere {
  constructor(renderer, camera) {
    this.renderer = renderer;
    this.camera = camera;
    this.scene = new THREE.Scene();

    const vert = `
      uniform float uTime;
      uniform float uEnergy;
      varying float vWaveHeight;

      void main() {
        float amplitude = 0.35;
        float frequency = 6.0;  
        
        // La velocidad base es 2.0. Con cada beat (uEnergy), 
        float velocity = 2.0 + (uEnergy * 25.0); 

        float angleY = (position.y * frequency) - (uTime * velocity);
        float wave = amplitude * sin(angleY);

        float angleX = (position.x * frequency * 0.8) - (uTime * velocity * 1.2);
        wave += (amplitude * 0.5) * sin(angleX);

        vWaveHeight = wave;
        vec3 newPosition = position + normal * wave;
        
        gl_Position = projectionMatrix * modelViewMatrix * vec4(newPosition, 1.0);
      }
    `;

    const frag = `
      uniform vec2 uMouse; 
      varying float vWaveHeight;

      void main() {
        vec2 m = uMouse * 0.5 + 0.5;
        vec3 black = vec3(0.02, 0.01, 0.03);
        vec3 midColor = mix(vec3(0.38, 0.0, 0.48), vec3(0.0, 0.6, 0.8), m.x);
        vec3 highColor = mix(vec3(1.0, 0.36, 0.0), vec3(0.1, 0.9, 0.2), m.y);

        float t = clamp(vWaveHeight + 0.5, 0.0, 1.0);
        vec3 col;
        if(t < 0.5) {
          col = mix(black, midColor, t * 2.0);
        } else {
          col = mix(midColor, highColor, (t - 0.5) * 2.0);
        }

        gl_FragColor = vec4(col, 1.0);
      }
    `;

    this.mat = new THREE.ShaderMaterial({
      vertexShader: vert,
      fragmentShader: frag,
      uniforms: {
        uTime:   { value: 0 },
        uEnergy: { value: 0 },
        uMouse:  { value: new THREE.Vector2(0, 0) }
      },
      wireframe: true 
    });

    this.geometry = new THREE.SphereGeometry(1.5, 128, 128);
    this.mesh = new THREE.Mesh(this.geometry, this.mat);
    this.scene.add(this.mesh);
  }

  setEnergy(v) { 
    this.mat.uniforms.uEnergy.value = v; 
  }

  update(time, mouse) {
    this.mat.uniforms.uTime.value = time;
    this.mat.uniforms.uMouse.value.set(mouse.nx, mouse.ny);
    this.mesh.rotation.y = time * this.mat.uniforms.uEnergy.value * 2; 
  }

  render() { this.renderer.render(this.scene, this.camera); }
}
``` 

## 6. Moodboard o referencias

REFERENCIAS STRUDEL: https://youtu.be/xAAbQMW0dFk

Principalmente DJ.DAVE y Switch-Angel.

MOODBOARD:

<img width="720" height="720" alt="download (1)" src="https://github.com/user-attachments/assets/2be210d7-89ae-44bb-94fe-aba6dbb4df8c" />
<img width="720" height="1280" alt="Particles2 png" src="https://github.com/user-attachments/assets/c19e99a2-8ac3-4ff3-aa0a-d795f64d9d30" />
<img width="736" height="414" alt="Backgrounds, Textured, Mobile Wallpaper" src="https://github.com/user-attachments/assets/7e4adbbc-5fee-4780-b886-7c07b4182448" />
<img width="736" height="736" alt="download (2)" src="https://github.com/user-attachments/assets/f40188b0-9612-4b3f-b146-dba9c85cf790" />


## 7. Bocetos

Inicialmente era 2D y solo morado y naranja:

<img width="1280" height="1119" alt="image" src="https://github.com/user-attachments/assets/fde3c4c3-f5e4-410c-8c3d-f720438c2029" />

Luego plantee la tranferencia de Oscillation a 3D:

<img width="1280" height="643" alt="image" src="https://github.com/user-attachments/assets/610f7fb8-7686-4503-a96b-1d9a2df5f67e" />


## 8. Mapa de decisiones



## 9. Mapa de presentación

Comentar que escogí Oscillation y random Gaussian como concepto del curso -> Explicar como lo transferí -> Mostrar en que parte se evidencia -> Mostrar el proyecto funcionando.

## 10. Uso explícito de IA como materializador

Todo el concepto visual, las decisiones de diseño, de cómo se supone que iba a reaccionar al audio, como quería el ritmo del audio, los colores, el mapeo con el mouse, eso lo decidí yo. Cuando prompteo, procuro que este sea el system instruction o en su defecto el primer promt: Eres mi tutor. Debo formar un aprendizaje perdurable, profundo y transferible. Guíame UN solo paso a la vez. Lento pero seguro. NO generes código por mí a menos que lo pida. Yo te diré que vamos a hacer, no generes cosas sin que las haya pedido.

En cuanto a la IA, la usé como apoyo porque nunca había usado three.js ni strudel, asi que le pedí el paso a paso de la intalación de librerías y la estructura base. Con eso entendí que Three tiene la scene, la camera, el renderer, la función animate que viene siendo como el draw en p5. También usé la IA para el vert y el frag, porque de eso se compone el shader y eso es algo que aun estoy aprendiendo.



## Bitácora de reflexión

# 🎮 Práctica: Simón Dice con React Native (Expo)

Implementación completa del juego de memoria **Simón Dice** desarrollado con React Native y Expo[cite: 1]. La aplicación genera una secuencia creciente de colores que la máquina ilumina uno por uno y que el jugador debe repetir en el mismo orden[cite: 1].

---

## 📋 1. Requisitos e Instalación

Para ejecutar este proyecto necesitas tener instalado **Node.js (versión LTS)**[cite: 1]. Puedes verificar tu instalación en la terminal con[cite: 1]:

```bash
node -v
npm -v
```

### 🚀 Instrucciones para correr el proyecto

1. **Clonar este repositorio e ingresar a la carpeta:**

   ```bash
   git clone <URL_DE_TU_REPOSITORIO>
   cd simon-dice
   ```

2. **Instalar las dependencias:**

   ```bash
   npm install
   ```

3. **Arrancar el servidor de desarrollo:**

   ```bash
   npx expo start
   ```

4. **Visualizar la aplicación:**
   - **En teléfono móvil:** Instala la app **Expo Go** (disponible en App Store para iOS o Play Store para Android), escanea el código QR que aparece en la terminal y la app se abrirá automáticamente[cite: 1].
   - **En computadora (Navegador Web):** Presiona la tecla `w` en la terminal después de ejecutar `npx expo start` para abrir la versión web[cite: 1].

---

## 🎯 2. Planteamiento del Problema y Reglas

### Descripción

Simón Dice es un juego de memoria donde la máquina muestra una secuencia creciente de colores iluminando los botones uno por uno (~500 ms encendido por botón), y el jugador debe repetirla tocando los botones en el mismo orden[cite: 1]. Cada ronda superada agrega un color más a la secuencia; si el jugador se equivoca, el juego termina y muestra el puntaje alcanzado[cite: 1].

### Requerimientos Funcionales Implementados

1. **Estado de la secuencia:** Se mantiene en estado la secuencia completa de colores generada por la máquina[cite: 1].
2. **Reproducción asíncrona:** Se reproduce la secuencia encendiendo cada botón el tiempo indicado sin bloquear la interfaz[cite: 1].
3. **Seguimiento del jugador:** Se lleva el control exacto de en qué posición de la secuencia va el jugador en cada toque[cite: 1].
4. **Validación:** Se compara cada toque del jugador contra el color esperado en esa posición[cite: 1].
5. **Control de turnos (Bloqueo):** Se distingue con un estado booleano si es turno de la máquina o del jugador para ignorar toques mientras la secuencia se está reproduciendo[cite: 1].
6. **Retroalimentación en pantalla:** Se muestra en todo momento si es turno de la máquina, turno del jugador, la ronda actual o el mensaje de _Game Over_ con el puntaje[cite: 1].

---

## 🧠 3. Diseño del Estado (`useState`)

Antes de codificar la lógica, se definieron los siguientes estados dentro de `SimonScreen` (`app/index.tsx`)[cite: 1, 2]:

| Estado             | Tipo de Dato     | Valor Inicial    | ¿Qué guarda y por qué se diseñó así?                                                                                                                                                                                                                                                                                  |
| :----------------- | :--------------- | :--------------- | :-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| **`sequence`**     | `string[]`       | `[]`             | **Secuencia de colores:** Es un arreglo de cadenas (`"red"`, `"green"`, `"blue"`, `"yellow"`)[cite: 1, 2]. Guarda en orden los colores generados por la máquina[cite: 1]. Además, su longitud (`sequence.length`) permite obtener la ronda actual y calcular el puntaje final sin crear estados redundantes[cite: 1]. |
| **`playerIndex`**  | `number`         | `0`              | **Posición actual del jugador:** Un entero que funciona como índice/puntero dentro de `sequence` (`sequence[playerIndex]`). Inicia en `0` cada ronda y suma `1` en cada acierto para validar paso a paso el orden de los toques del usuario[cite: 1].                                                                 |
| **`isPlayerTurn`** | `boolean`        | `false`          | **De quién es el turno:** Vale `false` cuando la máquina reproduce la secuencia (o cuando el juego no ha iniciado / terminó) y `true` cuando es turno del jugador[cite: 1]. Sirve como candado en `pressColor` para bloquear los botones durante la reproducción[cite: 1].                                            |
| **`activeColor`**  | `string \| null` | `null`           | **Botón iluminado:** Controla cuál de los 4 botones cambia su opacidad de `0.35` a `1` (`styles.padActive`), tanto en la demostración de la máquina como al presionar un botón[cite: 1, 2].                                                                                                                           |
| **`status`**       | `string`         | `"Toca empezar"` | **Estado visual del juego:** Muestra el texto superior indicando la ronda actual, de quién es el turno o el puntaje final al perder[cite: 1, 2].                                                                                                                                                                      |

### ⚙️ Nota técnica sobre la reproducción (`setTimeout` con retardo acumulado)

Para reproducir la secuencia sin bloquear el hilo principal de React Native, se recorre el arreglo `sequence` con `forEach` programando temporizadores (`setTimeout`)[cite: 1]. Dado que `forEach` es síncrono y registra todos los `setTimeout` de forma casi instantánea, **cada iteración utiliza un retardo acumulado distinto** calculado como `(index + 1) * 800` ms[cite: 1]. Así, cada botón se enciende durante `500 ms` y deja una pausa de `300 ms` antes del siguiente color, escalonando la animación en el tiempo y habilitando el turno del jugador únicamente cuando termina el tiempo total de la secuencia (`(currentSequence.length + 1) * 800` ms)[cite: 1].

---

## 👤 Autor

- **Maldonado Soria José Francisco**

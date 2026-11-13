# Tarea: Mi prompt avanzado

## Tarea elegida

Desarrollo integral de un juego web de PAC-MAN completo en un único archivo (HTML5, CSS3 y JavaScript), implementando una arquitectura MVC, motor de renderizado con Canvas, estética Pixel Art Neón adaptativa, sistema de menús navegables (Inicio, Dificultad, Pausa) y persistencia de puntuación/vidas mediante `localStorage`.

## Version 1: prompt basico

```text
Hazme un juego de Pacman en HTML, CSS y JavaScript que se vea bonito y se pueda jugar en el navegador.
```

* **Técnica agregada:** Ninguna (Prompt ingenuo/directo).
* **Por qué:** Es la forma inicial e intuitiva de pedir una tarea, pero carece de restricciones formales.
* **Qué mejoró en la respuesta:** La IA genera una versión genérica y muy básica de Pac-man, con código desorganizado en scripts mezclados y sin gestión de estados ni resiliencia ante errores.

## Version 2

```text
Actúa como un desarrollador de videojuegos frontend senior. Crea un juego de Pacman en un solo archivo HTML. Debes separar el código usando el patrón MVC. Incluye una pantalla de inicio donde se pueda elegir la dificultad (Fácil y Difícil) para cambiar la velocidad del fantasma, una pantalla de pausa al presionar la tecla ESC que diga "You are paused", y guarda el puntaje y las vidas en localStorage para que no se pierdan al recargar la página.
```

* **Técnica agregada:** *Role Prompting* (Desarrollador Senior) y *Descomposición de Requisitos* (Pantallas, MVC, LocalStorage).
* **Por qué:** Para evitar código "espagueti" y obligar a la IA a estructurar el flujo de pantallas y la persistencia de datos.
* **Qué mejoró en la respuesta:** La IA entregó un código estructurado en módulos orientados a objetos con funcionalidad de guardado, aunque la interfaz gráfica seguía siendo básica y el motor podía fallar en la detección de colisiones complejas.

## Version 3: prompt final

```text
Toma este rol: Eres un trabajador senior con amplia experiencia en desarrollo de videojuegos web. En tu trabajo anterior construiste un juego impecable y fuiste desafiado por tu jefe a un reto de un solo intento para crear el mejor juego de PAC-MAN en HTML5, CSS y JavaScript puro.

Reglas del desafío y arquitectura:
1. Formato: Entregar un único archivo HTML funcional con CSS incrustado y JS estructurado.
2. Arquitectura: Simular el patrón MVC (PacManModel, PacManView, PacManController).
3. Estética: Estilo CSS Pixel Art Neón con efectos de brillo y diseño responsive (adaptable a laptop y monitores).
4. Flujo de Interfaces (UI):
   - Pantalla 1: "Iniciar Partida".
   - Pantalla 2: "Selección de Dificultad" (Fácil y Difícil, variando la velocidad del fantasma).
   - Pantalla 3: "Pausa", activada con la tecla 'Esc', mostrando "You are paused" y reanudable con 'Enter' o botón "Continuar".
5. Persistencia: Guardar en localStorage la puntuación y las vidas restantes durante la partida.
6. Cero Bugs (Criterio de ejecución impecable): Como solo tienes 1 intento, optimiza el motor para garantizar 100% de estabilidad (por ejemplo, reducir a 1 fantasma táctico con IA de persecución en vez de 4 inestables).

Genera el código HTML completo con la simulación de carpetas y clases JS bien nombradas.
```

* **Técnica agregada:** *Prompt Estructurado* + *Autocrítica / Restricción de Calidad (Single-attempt Optimization)* + *Chain of Thought Implícito*.
* **Por qué:** Se requería garantizar cero fallos de ejecución en la entrega y una UI con acabados profesionales.
* **Qué mejoró en la respuesta:** La IA entregó una aplicación robusta, con interfaz tipo neón retro de alto nivel, motor Canvas libre de bugs y un manejo impecable de eventos de teclado y menús contextuales.

## Tecnicas usadas en me prompt final

| Técnica de Prompting | Identificación en el Prompt Final |
| --- | --- |
| **Role Prompting** | *"Eres un trabajador senior con amplia experiencia en desarrollo de videojuegos web... desafiado a un reto de un solo intento..."* |
| **Prompt Estructurado** | Uso de secciones numeradas explícitas: *1. Formato, 2. Arquitectura, 3. Estética, 4. Flujo de Interfaces, 5. Persistencia, 6. Cero Bugs.* |
| **Descomposición** | División funcional de la UI en 3 pantallas independientes (Inicio, Dificultad, Pausa) y del código en 3 capas (Modelo, Vista, Controlador). |
| **Autocrítica / Restricción** | *"Como solo tienes 1 intento, optimiza el motor para garantizar 100% de estabilidad (por ejemplo, reducir a 1 fantasma táctico...)"* |

## Evaluacion del resultado

| Criterio de Evaluación | Cumple (Sí / No) | Observaciones |
| --- | --- | --- |
| 1. ¿El código se encuentra en un único archivo ejecutable en cualquier navegador? | **Sí** | Todo el HTML, CSS (Neón) y JavaScript están integrados en un solo archivo web. |
| 2. ¿Implementa el patrón MVC de manera clara en JavaScript? | **Sí** | Se definen las clases `PacManModel`, `PacManView` y `PacManController`. |
| 3. ¿Posee pantallas de Inicio, Selección de Dificultad y Pausa interactiva con 'Esc'? | **Sí** | Flujo completo de modales funcionales con respuesta a eventos de teclado (`ESC` y `ENTER`). |
| 4. ¿Guarda la puntuación y vidas en `localStorage` ante una recarga de página? | **Sí** | El modelo almacena y restaura el estado mediante `localStorage` automáticamente. |

## Por que elegi estas tecnicas

Elegí la combinación de **Role Prompting**, **Prompt Estructurado**, **Descomposición** y **Autocrítica / Restricción** porque el desarrollo de juegos en un solo archivo web requiere balancear estética compleja, control de estados de interfaz y lógica de bucle continuo sin colapsar el código. El *Role Prompting* elevó el estándar cualitativo del diseño CSS; la *Descomposición* aseguró que cada capa arquitectónica (MVC y pantallas UI) quedara aislada; y la *Autocrítica/Restricción* evitó que la LLM intentara abarcar mecánicas redundantes de múltiples fantasmas que habrían introducido bugs de colisión, priorizando un motor 100% fluido y estable a la primera ejecución.
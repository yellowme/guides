Code Review
===========

Una guia para realizar `code reviews` y programar anticipando la revisión de tu código.

*Nota:* Si estás en desacuerdo con algún lineamiento, abre un *issue* en este repositorio, los debates en los `code reviews` nutren más estos lineamientos.

* [Generales](#generales)
* [Lenguajes](#lenguajes)
* [Solicitar una revisión](#solicitar-un-code-review)
* [Revisión](#revision)
  * [Revisores](#revisores)
  * [Comentar](#comentar)
  * [Responder](#responder)
* [Escenarios](#escenarios)

Generales
--------

* Aceptar que las deciciones de programación son `opiniones`.
  * Dialoga con tus compañeros, tomen una decisión rápida y precisa.
* **Pregunta, no exijas**. Evitamos hacer juicios y suposiciones sobre la perspectiva del autor.
  * ¿Qué piensas de nombrar esto como `userId`?)
* **Pide aclaracione**s si no entiendes un comentario sobre tu código.
* **Evita**r el uso de terminos como:
  * Mi código, tú código, ese código no es mío.
  * Estúpido, tonto, etc.
* Sé **explícito**, *recuerda* que las personas no siempre entenderán tu perspectiva y contexto.
* Sé **humilde**.
* **No** uses el sarcasmo. 
* El uso inteligente de *emojis* queda a tu consideración.
* **Agrega ligas** directas a `commits, ramas, posts, etc`. Tu compañero te lo agradecerá.

Lenguajes
--------------

* [Kotlin](./KOTLIN.md)

Solicitar un `code review`
-------------------------

> No lo tones personal. La revisión es sobre el código, no sobre tu persona. Es muy fácil mal interpretar la retroalimentación. Intenta leer los comentarios como clarificaciones a dudas libres de agresividad.

* **Agradece** los comentarios de tus compañeros.
* **Explicar** por qué una porción de código existe y cual es su intención, incluso realiza preguntas a tu compañero.
* **Agrega/Comparte** la liga del `pull request` a tus compañeros de equipo.
* **Entiende** la perspectiva de tu compañero.
* **Responde** cada comentario.

Revisión
--------------

### Revisores

Lo primero es entender los cambios realizados:

* Solución de errores (bugs).
* Mejorar la experiencia de usuario.
* *Refactor* a código existente.

Luego

* **Identifica** maneras de simplificar el código sin remover funcionalidad.
* **Ofrece** implementaciones alternativas pensando en que el autor puede ya haber considera tu alternativa. *Recuerda ser educado y a preguntar de forma correcta*
* **Entender** la perspectiva del autor.
* **Aprueba** el `pull request`. Ya sea con acciones que brinde la plataforma o un comentario mencionando al autor.
* Puedes hacer uso de :thumbsup: o felicitaciones cuando tu compañero haya hecho un trabajo más allá lo imaginable.

### Comentar

Al comentar se debe hacer referencia a los lineamientos definidos en este repositorio

    [Guideline Name](www.ourawesomerepo.com):

    > Nombramiento de varialbes

### Responder

Ejemplo de un buen comentario de respuesta es:

    Tienes razón. Muchas gracias. Arreglado en a4994ec

*Nota:* Agregá la liga directa al *commit* donde hiciste actualizaciones, recuerda que todos tenemos otros pendientes o tareas asignadas.

Escenarios
--------------

* En caso de que existan muchos comentarios de peticiones de aclaración o soluciones alternativas. 
  * Hablen de forma directa por chat, en persona, compartiendo la pantalla, etc.
* Si un debate se vuelve muy filosófico o académico... 
  * muevan la discusión de forma personal en alguna charla de los Viernes.

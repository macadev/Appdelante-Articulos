<p align="center">
  <img src="https://github.com/macadev/Appdelante-Articulos/blob/master/imagenes/puertos_banner2.png"/>
</p>

# ¿Qué son los puertos?

Probablemente has escrito algo similar a lo siguiente cientos de veces:

```
var express = require('express')
var app = express()

app.listen(3000, function() {
	console.log("Escuchando en el puerto 3000")
})
```
¿Alguna vez te has preguntado qué es ese número 3000? Al hacerle esa pregunta a algunos amigos, todos respondieron algo como lo siguiente:

> "Es un puerto. Tiene algo que ver con el networking. En el caso de un servidor web, necesitas escuchar en un puerto para que puedas recibir mensajes de HTTP."

Luego le hice la pregunta a mis colegas en la oficina, y tampoco me pudieron dar una respuesta concreta. ¡Cómo es posible que todos usemos puertos en nuestro día a día sin saber qué son!

Por esta razón decidí escribir este artículo. Vamos a explorar en detalle qué son los puertos, con el enfoque de responder las siguientes preguntas:

- ¿Un puerto es una abstracción de software, o algo físico en mi hardware?
- ¿Cuantos puertos existen en mi computadora?
- ¿Por qué es común elegir el puerto número 3000 cuando escribimos código?
- *¿Realmente qué es un puerto?*

## El origen de los puertos

Repasemos por un momento el concepto más fundamental detrás del internet: **las direcciones**. 

Todo en la web tiene una dirección, justo como en tu ciudad cada edificio tiene una dirección. Esta abstracción es lo que permite al servicio de correo hacer llegar cartas y paquetes a tu casa. La web funciona exactamente igual. Todos los dispositivos, desde smartphones hasta laptops y servidores, tienen una dirección de IP que es usada para hacerles llegar paquetes de información. El servidor de Appdelante usa tu dirección de IP para mandarte los paquetes que contienen la página web que estás leyendo en este instante.

Hace mucho tiempo, las computadoras solo podían correr un programa a la vez. Eso significaba que con solo tener la dirección IP de otro dispositivo, podías hacerle llegar paquetes de información a ese único programa.

<p align="center">
  <img src="https://github.com/macadev/Appdelante-Articulos/blob/master/imagenes/ports_explained.png"/>
</p>

A medida que las computadoras se hicieron más rápidas, surgió la posibilidad de correr muchos programas al mismo tiempo. No solo eso, sino que todos ellos pueden estar enviando paquetes al internet simultáneamente. Por ejemplo, en tu navegador puedes estar leyendo este artículo mientras tienes a la aplicación de WhatsApp abierta en tu Desktop. De aquí surge una pregunta bastante sencilla:

> ¿Cómo nos aseguramos que los paquetes destinados a WhatsApp lleguen a ella y los paquetes destinados al navegador lleguen a él?

¡De aquí surge la la necesidad de los puertos! Eventualmente los ingenieros trabajando en la web se encontraron con este problema. Su respuesta fue inventar el concepto de un puerto, que es simplemente una abstracción de software que utilizamos para poder determinar quién debe obtener un paquete. El diagrama de abajo muestra a una laptop hablando con un servidor web a través de unos puertos:

<p align="center">
  <img src="https://github.com/macadev/Appdelante-Articulos/blob/master/imagenes/ports_explained_2.png"/>
</p>

Sin el concepto de un puerto tu sistema operativo no sabría a qué proceso entregarle un paquete que llega por la red. Es como si llegara una carta a un edificio sin indicar el número del apartamento al que está destinada. ¿Cómo sabría el cartero a quién entregarle la carta?

## ¿Cómo son asignados los puertos?

Los puertos son asignados por el sistema operativo de tu dispositivo cada vez que un proceso va a hacer un pedido por el internet. Por ejemplo, cuando abres una página web, tu navegador le pide al sistema operativo un puerto para poder recibir la respuesta del pedido que va a enviar. Luego de recibir la respuesta, el navegador libera el puerto para que el sistema operativo lo pueda volver a asignar.

El número de puerto que te otorga el sistema operativo es aleatorio. Sin embargo, hay algunos puertos que son reservados por el sistema operativo para ciertos protocolos y usos.

## ¿Cuáles son los puertos reservados?

Los puertos reservados son también llamados "puertos bien conocidos". Son todos aquellos que van de 0 a 1024. Estos son algunos números que a lo largo de la historia se asociaron con ciertos protocolos y se volvieron el "estándar". Algunos ejemplos:

- Los servidores web escuchan en el puerto 80. Este es el puerto usado comúnmente para el protocolo HTTP.
- Si el tráfico web es seguro, se utiliza el puerto 443 para el protocolo HTTPS.
- El puerto 22 es comúnmente usado para establecer conexiones por SSH.

Es importante denotar que los protocolos que utilizan los puertos reservados también funcionarían con puertos normales, pero adoptar estándares nos quita un dolor de cabeza a todos los programadores. Imagina si no hubiese un puerto estándar para HTTP. Tendrías que escribir URLs como www.google.com:56799 para accesar servidores en el puerto que están escuchando. ¡Eso sería horrible!

## ¿Por qué los puertos reservados necesitan "root access" (sudo)?

Si tomas el código que está al principio de este artículo, cambias el puerto de 3000 a 80 y lo corres con Node.js, verás el siguiente error:

```
$ node app.js
Error: listen EACCES 0.0.0.0:80
    at Object._errnoException (util.js:1041:11)
    at _exceptionWithHostPort (util.js:1064:20)
    ...
```

¡Pero si corres el código con `sudo` verás que funciona!


```
$ sudo node app.js
Escuchando en el puerto 80
```

¿A qué se debe esto? Pues se trata de una medida de seguridad que ha existido en sistemas operativos como Unix desde los años 80. En esa época, no existían las computadoras personales. En su lugar, existían computadoras a las que muchísimos usuarios se podían conectar al mismo tiempo. En muchos casos no era posible confiar en todos los usuarios (¡puede que alguno tuviese malas intenciones!). Hacer que los "puertos bien conocidos" requieran "root access" significa que solo el administrador puede correr procesos en ellos. 

Imagina si un usuario malévolo tratase de cambiar el proceso para SSH por el puerto 22 con uno que robase contraseñas. ¡No podría hacerlo! Solo el administrador tiene root access. Hoy en día esta medida de seguridad es un poco obsoleta, porque ahora todos tenemos nuestras propias computadoras. ¿Cuándo fue la última vez que te conectaste a un servidor compartido? Quizás en tu días en la universidad, pero aún así no es algo muy común.

## ¿Cuantos puertos tiene mi computadora?

Los números de puerto están compuestos por 16 bits y no son signados, lo cual significa que tu sistema operativo puede asignar 2^16 - 1 = 65,535 puertos.

## ¿Eso significa que mi computadora solo puede abrir 65,535 conexiones simultáneas?

¡No! Aquí hay un detalle crucial. El número de conexiones que tu computadora puede establecer no está relacionado al número de puertos. Piensa por ejemplo en un servidor web escuchando en el puerto 80. Es posible que miles de computadoras estén hablando con ese servidor. No es raro escuchar de servidores web que son capaces de procesar 100,000 conexiones simultáneas, todas en un solo puerto.

Al principio esto te parecerá difícil de entender. Asegúrate de separar en tu cabeza las ideas de puertos y conexiones. ¡Son dos cosas totalmente distintas!

## ¿Por qué el puerto 3000 es comúnmente usado?

¡Por ninguna razón en particular! Es una decisión arbitraria que se popularizó porque está presente en la documentación de frameworks como Express en Node.js.

## Conclusión

Como puedes ver, los puertos no son más que una abstracción para permitir a muchos procesos enviar y recibir paquetes simultáneamente. Espero haber resuelto tus dudas, pero si algo no está claro deja un comentario abajo y te responderé.

Si te gustó este artículo, la mejor manera de apoyarnos es compartiéndolo con tus amigos. ¡Déjanos saber tu opinión! ¿Te interesa aprender más sobre el networking?

# ¿Qué son los puertos?

Probablemente has escrito algo similar a lo siguiente cientos de veces:

```
var express = require('express')
var app = express()

app.listen(3000, function() {
	console.log("Escuchando en el puerto 3000")
})
```

Si alguien te dijera lo siguiente, ¿Qué responderías?


> "Oh gran programador [Inserta tu nombre aquí], puedes explicarme que son los puertos?"

Mi respuesta hace unos años hubiese sido algo como lo que sigue:

> "Hmmm, tiene algo que ver con el networking. En el caso de un servidor web necesitas escuchar en un puerto para que puedas recibir mensajes de HTTP"

¿Cuál es le problema con esta respuesta? No respondí la pregunta! Solo dije algo genérico acerca de los servidores web y el networking. La persona que me hiciese esta pregunta probablemente esperaría que yo hablara de lo siguiente:

- ¿Es una abstracción de software o acaso los puertos son algo físico en mi hardware?
- ¿Cuantos puertos existen en mi computadora?
- ¿Por qué elegimos el numero 3000 comúnmente cuando desarrollamos?
- *¿Realmente qué es un puerto?*

Dejemos de hablar de conversaciones inventadas y respondamos estas preguntas!

## ¿De dónde surge este concepto?

Repasemos por un momento el concepto más fundamental de la internet: **las direcciones**. 

Todo en la web tiene una dirección, justo como en tu ciudad cada casa y negocio tiene una dirección. Esta abstracción que inventamos los humanos es lo que permite al servicio de correo hacer llegar cartas y paquetes a tu casa. La web funciona exactamente igual. Todos los dispositivos desde smartphones hasta laptops y servidores tienen una dirección de IP. El servidor de appdelante usa tu dirección de IP para hacerte llegar unos paquetes conteniendo esta página web que estas leyendo!

Hace mucho tiempo, las computadoras solo podían correr un programa a la vez. Esto significa que con solo tener la dirección IP de otro dispositivo podías hacerle llegar paquetes de información a ese único programa.

----                                        ----
Dibujo de dinosaurio usando          Otro dinosaurio
computadora vieja        --------->  (160.168.1.101)
   (192.168.1.101)
---                                         ----

A medida que las computadoras se hicieron más rápidas surgió la posibilidad de correr muchos programas al mismo tiempo. No solo eso, sino que todos ellos pueden estar enviando paquetes al internet simúltaneamente. Por ejemplo, en tu browser puedes estar leyendo este artículo mientras tienes a la aplicación de WhatsApp abierta en tu Desktop. De aquí surge una pregunta bastante sencilla:

> ¿Cómo nos aseguramos que los paquetes destinados a WhatsApp lleguen a ella y los paquetes destinados al Browser lleguen a el?

¡De aquí surge la abstracción de los puertos! Eventualmente los ingenieros trabajando en la web se encontraron con este problema. Su respuesta fue inventar el concepto de un puerto, que es simplemente una ficción que utilizamos para poder determinar quien debe obtener un paquete. Ahora nuestro diagrama se ve así:

----                                        ----
Dibujo de dinosaurio usando              Servidor
computadora vieja        --------->  (160.168.1.101:3000)
   (192.168.1.101:3500)
---                                         ----

El siguiente ejemplo te va ayudar a entender como tu computadora usa los puertos:

* En tu navegador escribes wwww.appdelante.com
* Un pedido es preparado para ser enviado al servidor de Appdelante. Este pedido incluye:
	*  Tu dirección IP: 192.168.1.101
	*  Un numero de puerto que te otorga el sistema operativo de forma aleatoria. Por ejemplo: 53672
*  El pedido viaja por la internet al servidor de Appdelante (aqui hay muchos detalles que no voy a explicar en este artículo)
*  El servidor obtiene el paquete y prepara una respuesta, la cual indica que la combinación de tu IP y puerto son la destinación.
*  La respuesta viaja por el internet a tu dispositivo.
*  Tu sistema operativo empieza a procesar la respuesta y se da cuenta hay un programa esperando una respuesta en el puerto 53672, justo el que espicifica el paquete que acaba de arrivar.
*  El sistema operativo le pasa el contenido del paquete a tu browser.
*  Tu browser te muestra el home page de Appdelante.
*  El puerto que tu browser uso para hacer el pedido es liberado. El sistema operativo ahora se lo puede asignar a otro programa que quiera comunicarse por la internet. 










# Web cache poisoning

## Cache Busting

Cache busting is useful because it allows your visitors to receive the most recently updated files without having to perform a hard refresh or clear their browser cache: en mis palabras es como lo que rompe la cache en este caso se usa el Query string ?cb=1234

> https://www.keycdn.com/support/what-is-cache-busting

## Web cache poisoning with an unkeyed header


Para este laboratorio primero revismos que la pagina principal maneja una header 

![image](https://user-images.githubusercontent.com/63270579/199336600-db300a36-f0be-46bc-b5a0-bc450656f1ff.png)

La X-Cache cuando logramos escribir en la cache nos manda hit por lo tanto mandamos eso al repeater

![image](https://user-images.githubusercontent.com/63270579/199336871-23dfa0c6-ac17-482d-9f30-2d833d5de5d0.png)

 Agregamos la cabecera X-Forwarded y el parametro ?cd=1234 para ver si podemos infectar la cache pero aparte nos damos cuenta
 que la pagina genera una ruta con la cual carga un script en base a donde viene y se puede controlar con la X-Forwarded...
 
 Para simular que usuario entra y le ejecutamos un script con la cache envenenada quitamos los parametros de get y despues vamos al exploit server
 para finalmente poner la url y direccion del archivo que es la misma que aparece en la solicitud solo le vamos a poner la direccion del 
 exploit server.
 
 ![image](https://user-images.githubusercontent.com/63270579/199347595-73e0b5a2-de4d-47eb-98a6-4e5bb681abe9.png)

Con esto y hay que probar varias veces al usuario que hace ese reques le estariamos ejecutando un JS lo cual podriamo robar su cookie en este 
caso solo mostramos las cookies y se resuleve el lab.



## Lab: Web cache poisoning with an unkeyed cookie

Para este lab primero vemos las peticiones y respuestas nos damos cuenta de que posiblemente se puede envenenar la cache.


![image](https://user-images.githubusercontent.com/63270579/213589920-c6f91780-f90a-4d78-b840-a379757a9186.png)

Nos damos cuenta que se refleja lo que pongamos en la cabecera entonces ponemos un alert pero lo que si no se porque se usan los "-alert(1)-" porque si no usas los guines y las comillas no jala. sabemos que funciona porque donde dice cache en la respuesta (X-Cache: hit) dice hit en vez de miss. y asi se resuelve el lab si tu visitas la pagina te das cuenta que si ejecuta el alert pero portswingger emula que un usuario entra a la pagina no es necesario.


## Web cache poisoning with multiple headers

Para este lab se resolvio de la siguiente manera ( que esta bien dificil algo asi en la realidad). Ponemos un cache buster y si ponemos la X-Forwarded-Host vemos que no afecta la respuesta 

![image](https://user-images.githubusercontent.com/63270579/213619636-76787b80-40c1-421b-90b8-d7869a75b7f8.png)


Pero ahora si ponemos X-Forwarded-Scheme: nohttp nos dice que redirecciona nos mand aun 302. Aqui es donde reside la vulnerabilidad.

![image](https://user-images.githubusercontent.com/63270579/213619939-c90e57f7-4435-4bb7-b5c6-54f9fb2dc79f.png)

Porque redirecciona a la pagina que esta en Host y lo que tiene en la peticion de GET. Ahora se pone la X-Forwarded-Host: con la url de exploit server. no sin antes modificar los valores y ponerle alert(document.cookie).

![image](https://user-images.githubusercontent.com/63270579/213620419-d5a05f66-940f-4494-8dd4-918cf0e6fbd5.png)

Ahora ponemos la direccion del exploit server en el X-Forwarded-Host:

![image](https://user-images.githubusercontent.com/63270579/213620670-13449344-a43c-454d-bc6a-1e2f3a73284a.png)


Se puso la url del exploit server y se ve que se envenena la cache con hit revisamos la respuesta en el navegador y deberia de mostrar el alert(document.cookie).

![image](https://user-images.githubusercontent.com/63270579/213620950-6fab53a5-2598-429c-8280-5775477bca71.png)

Quitarmos el buster y ya vemos que va a redirigir entonces envenenamos el cache y se simula que un usurio entra y la chace entrega la respuesta de redireccion y 
se ejecuta el payload.













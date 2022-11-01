# Web cache poisoning


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























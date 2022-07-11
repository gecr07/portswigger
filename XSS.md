# Cross Site Scripting

Existen basicamente 3 tipos de XSS

# XSS reflejado (Reflected cross-site scripting)
Donde el script malicioso proviene de la solicitud HTTP actual.

> XSS Reflejado: la aplicación utiliza datos sin validar, suministrados por un usuario y codificados como parte del HTML o
> JavaScript de salida. Un ejemplo de este tipo de XSS podría ser, si al introducir código JavaScript 
> en el buscador de una página, este código es ejecutado en el navegado


# XSS almacenado (Stored cross-site scripting)

Donde el script malicioso proviene de la base de datos del sitio web. 

> XSS Almacenado: la aplicación almacena datos proporcionados por el usuario sin validar ni sanear, los que posteriormente son visualizados por otro usuario o un administrador. Una página web sería vulnerable a este tipo de XSS si por ejemplo un usuario introduce como dirección de entrega código JavaScript 
> y este código se ejecuta cuando un empleado accede al perfil del usuario.

# XSS basado en DOM(DOM-based cross-site scripting)

Donde la vulnerabilidad existe en el código del lado del cliente en lugar del código del lado del servidor.


> XSS Basados en DOM: la aplicación procesa datos controlables por el usuario de forma insegura.  De forma similar al XSS Reflejado, un ejemplo de este ataque sería si en la URL escribimos código JavaScript y la web tiene un script que añade la URL sin sanear como parte del HTML. Al cargar la web el código JavaScript se ejecutará. La diferencia de este tipo de XSS con el XSS reflejado es que si miramos el código no veremos el JavaScript directamente en el HTML ya que todo sucede en el DOM.


# Ejemplos Laboratorios

## Reflected XSS into HTML context with nothing encoded

En este caso el payload mas basico es 

><script>alert(1)</script>

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
  
  
  
## Referencias

> https://portswigger.net/web-security/cross-site-scripting


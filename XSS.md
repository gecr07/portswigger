# Cross Site Scripting

Existen basicamente 3 tipos de XSS

# XSS reflejado
Donde el script malicioso proviene de la solicitud HTTP actual.

> XSS Reflejado: la aplicación utiliza datos sin validar, suministrados por un usuario y codificados como parte del HTML o
> JavaScript de salida. Un ejemplo de este tipo de XSS podría ser, si al introducir código JavaScript 
> en el buscador de una página, este código es ejecutado en el navegado


# XSS almacenado

Donde el script malicioso proviene de la base de datos del sitio web. 

> XSS Almacenado: la aplicación almacena datos proporcionados por el usuario sin validar ni sanear, los que posteriormente son visualizados por otro usuario o un administrador. Una página web sería vulnerable a este tipo de XSS si por ejemplo un usuario introduce como dirección de entrega código JavaScript 
> y este código se ejecuta cuando un empleado accede al perfil del usuario.

# XSS basado en DOM

Donde la vulnerabilidad existe en el código del lado del cliente en lugar del código del lado del servidor.


> XSS Basados en DOM: la aplicación procesa datos controlables por el usuario de forma insegura.  De forma similar al XSS Reflejado, un ejemplo de este ataque sería si en la URL escribimos código JavaScript y la web tiene un script que añade la URL sin sanear como parte del HTML. Al cargar la web el código JavaScript se ejecutará. La diferencia de este tipo de XSS con el XSS reflejado es que si miramos el código no veremos el JavaScript directamente en el HTML ya que todo sucede en el DOM.


# Ejemplos Laboratorios

## Reflected XSS into HTML context with nothing encoded

En este caso el payload mas basico es 

><script>alert(1)</script>




## Referencias

> https://portswigger.net/web-security/cross-site-scripting


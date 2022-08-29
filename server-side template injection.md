
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
























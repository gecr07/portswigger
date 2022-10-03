

# Manipulating WebSocket messages to exploit vulnerabilities

Para este lab

1. Primero vamos a Live Chat y podemos ver como viajan los datos porque este funciona con un websocket
2. cuando pulsamos send e interceptamos pues nos deja ver que manda la info asi

```
{"message":"masa&gt;"}

```
3. Si nos fijamos un poco en el codigo al parecer hace url encode de hecho si metemos el caracter <> ya cuando manda la peticion que es interceptada por el burp ya
hizo ese proceso por lo cual esta como sanitizando elw codigo ponemos el payload de XSS y se resuleve.

```
{"message":"<img src=1 onerror='alert(1)'>"}

```


























































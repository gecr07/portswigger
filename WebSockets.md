# Que son los Websockets

WebSockets are a bi-directional, full duplex communications protocol initiated over HTTP. They are commonly used in modern web applications for streaming data and other asynchronous traffic.

## How are WebSocket connections established?

WebSocket connections are normally created using client-side JavaScript like the following:

```
var ws = new WebSocket("wss://normal-website.com/chat");
```

The wss protocol establishes a WebSocket over an encrypted TLS connection, while the ws protocol uses an unencrypted connection.

## Handshake Websockets

To establish the connection, the browser and server perform a WebSocket handshake over HTTP. The browser issues a WebSocket handshake request like the following:

First 

```
GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket

```

If the server accepts the connection, it returns a WebSocket handshake response like the following:

```

HTTP/1.1 101 Switching Protocols
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Accept: 0FFP+2nmNIf/h+4BP36k9uzrYGk=


```
At this point, the network connection remains open and can be used to send WebSocket messages in either direction

## Explicacion de las cabeceras de Websockets

Several features of the WebSocket handshake messages are worth noting:

1. The Connection and Upgrade headers in the request and response indicate that this is a WebSocket handshake.

2. The Sec-WebSocket-Version request header specifies the WebSocket protocol version that the client wishes to use. This is typically 13.
 
3. The Sec-WebSocket-Key request header contains a Base64-encoded random value, which should be randomly generated in each handshake request.
4. The Sec-WebSocket-Accept response header contains a hash of the value submitted in the Sec-WebSocket-Key request header, concatenated with a specific string defined in the protocol specification. This is done to prevent misleading responses resulting from misconfigured servers or caching proxies.

## Javascript

```
ws.send("Peter Wiener");



```


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

# Lab: Manipulating the WebSocket handshake to exploit vulnerabilities

Primero abrimos la pagina y vemos que usa web sockets si intentamos el ataque del ejercicio pasado nos banea la direccion para 
falsificar nuestra ip se usa:

```
X-Forwarded-For: 1.1.1.1

```
 Antes usaste el intercept y mandaste esa peticion al repeater
 
 Como esta detectando nuestro ataque hay dos metodos
 
 
 ```
 {"message":"<img src=1 oNeRrOr=alert`1`>"}
 {"message":"<iframe src='jAvAsCrIpT:alert'1''>"}
 
 ```























































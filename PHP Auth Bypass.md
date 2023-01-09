






# PHP Type Juggling Vulnerabilities


La vulnerabilidad radica en que PHP antes de comparar una condicion trata de convertir esas variables en un tipo comun

> PHP has a feature called “type juggling”, or “type coercion”. This means that during the comparison 
of variables of different types, PHP will first convert them to a common, comparable type.


```
$example_int = 7
$example_str = “7”
if ($example_int == $example_str) {
   echo("PHP can compare ints and strings.")
}

```

> The code will run without errors and output “PHP can compare ints and strings.” 
This behavior is very helpful when you want your program to be flexible in dealing with different types of user input.

Este comportamiento tambien lo he visto en codigo de Js tendriamos que investigar.


## Explotacion Ejemplo


> For example, when PHP needs to compare the string “7 puppies” to the integer 7,
PHP will attempt to extract the integer from the string. So this comparison will evaluate to True.


```
(“7 puppies” == 7) -> True
```

## Ejemplo 2 

> But what if the string that is being compared does not contain an integer? 
The string will then be converted to a “0”. So the following comparison will also evaluate to True:

```
(“Puppies” == 0) -> True

```

## Ejemplo 3

The most common way that this particularity in PHP is exploited is by using it to bypass authentication.

Let’s say the PHP code that handles authentication looks like this:


```
if ($_POST["password"] == "Admin_Password") {login_as_admin();}
```

Then, simply submitting an integer input of 0 would successfully log you in as admin, since this will evaluate to True:

```
(0 == “Admin_Password”) -> True
```



## EchoCTF  6letter-juggler

Para este lab nos damos cuenta de varias cosas 1 esta usanod PHP como se puede ver en la siguiente imagen 2 el nombre del reto podriamos buscarlo.








### Referencias

> https://medium.com/swlh/php-type-juggling-vulnerabilities-3e28c4ed5c09

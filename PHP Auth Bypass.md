






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



![image](https://user-images.githubusercontent.com/63270579/211359923-b913c640-ed10-4bad-928b-9e3fd461e8a9.png)


Si buscabamos asi el ataque 

![image](https://user-images.githubusercontent.com/63270579/211360201-920d462e-616d-41c1-8fe7-724bf498e9fc.png)


Para bypasear el login se mando las variables como array [].

![image](https://user-images.githubusercontent.com/63270579/211371108-2d865595-06e3-4b5e-8a2b-92a2ae351472.png)

![image](https://user-images.githubusercontent.com/63270579/211371229-7aa2c4d9-215a-483c-b31b-ef347b6e8345.png)

```
Looking at the title (which you definitely should take a closer look at along with the description in CTFs), it's pretty obvious that we're dealing with PHP type juggling vulnerability.

We can try some basic payloads related like to it like 0 but we will reach to a dead end eventually, but there's still one thing which we should try, what if we submitted our parameters as arrays instead of strings?

Submitting ?login[]=John&password[]=Doe will bypass the whole login process and we can get our flag from there, interestingly a warning will pop-up: Warning: strcmp() expects parameter 1 to be string, array given in /var/www/html/index.php on line 16, so it's safe to assume that the back-end PHP code looks like this:

if (strcmp($_GET['login'], 'unknownuser') == 0 && strcmp($_GET['password'], 'unknownpassword') == 0) { // do stuff as authenticated user }

The comparison will result in a NULL since an array can't be compared to a string, and due to type juggling NULL equals 0 (which satisfies the condition).

For further reading: https://owasp.org/www-pdf-archive/PHPMagicTricks-TypeJuggling.pdf

```




### Referencias

> https://medium.com/swlh/php-type-juggling-vulnerabilities-3e28c4ed5c09

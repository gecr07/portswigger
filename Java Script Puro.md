# JS 

## Eventos de JS 

> Cuando un dispositivo crea un evento, el programa puede escucharlo para saber cuándo llevar a cabo un comportamiento específico.
> Un detector de eventos es una función que inicia un proceso predefinido si ocurre un evento específico. Entonces, un detector de eventos "escucha" una acción, luego llama a una función que realiza una tarea relacionada. Este evento puede tomar una de muchas formas. Los ejemplos comunes incluyen eventos de mouse, eventos de teclado y eventos de ventana.

> https://www.makeuseof.com/javascript-event-listeners-how-to-use/

## addEventListener

> Puede escuchar eventos en cualquier elemento del DOM . JavaScript tiene una función addEventListener() a la que puede llamar en cualquier elemento de una página web. La función addEventListener() es un método de la interfaz EventTarget . Todos los objetos que admiten eventos implementan esta interfaz. Esto incluye la ventana, el documento y los elementos individuales de la página.

Estructura Basica

```
element.addEventListener("event", functionToExecute);

```

Donde: the element can represent any HTML tag (from a button to a paragraph),the “event” is a string naming a specific, recognized action and the functionToExecute is a reference to an existing function.

***Ejemplo***

```
<body>

<h2>JavaScript addEventListener()</h2>

<p>This example uses the addEventListener() method to attach a click event to a button.</p>

<button id="myBtn">Try it</button>

<p id="demo"></p>

<script>
document.getElementById("myBtn").addEventListener("click", displayDate);

function displayDate() {
  document.getElementById("demo").innerHTML = Date();
}
</script>

</body>
</html> 
```
> https://www.w3schools.com/js/js_htmldom_eventlistener.asp

NOTA: Se pueden capturar cualquier evento generado por el usuario mousemove keyup doble click etc.

## Enviar mensajes  postMessage()

> postMessage() method safely enables cross-origin communication between Window objects; e.g., between a page and a pop-up that it spawned, or between a page and an iframe embedded within it

## eval()

> The eval() method evaluates or executes an argument.

```
<!DOCTYPE html>
<html>
<body>

<h1>JavaScript Global Methods</h1>
<h2>The eval() Method</h2>

<p id="demo"></p>

<script>
let x = 10;
let y = 20;
let text = "x * y";
let result = eval(text);

document.getElementById("demo").innerHTML = result;
</script>

</body>
</html>
```

Es un riesgo usar esta funcion de un input arbitrario.

## indexOf() 

El método indexOf() retorna el primer índice en el que se puede encontrar un elemento dado en el array, ó retorna -1 si el elemento no esta presente.

## XMLHttpRequest

Sirve para hacer peticiones sincronas o asincronas a una url el codigo basico es:
> XMLHttpRequest es un objeto nativo del navegador que permite hacer solicitudes HTTP desde JavaScript.
A pesar de tener la palabra “XML” en su nombre, se puede operar sobre cualquier dato, no solo en formato XML. Podemos cargar y descargar archivos, dar seguimiento y mucho más.

```
// Creamos un nuevo XMLHttpRequest
var xhttp = new XMLHttpRequest();

// Esta es la función que se ejecutará al finalizar la llamada
xhttp.onreadystatechange = function() { // Ay que usar esto para saber cuando esta lista
  // Si nada da error
  if (this.readyState == 4  && this.status == 200) {//this.readyState == 4 xhr.readyState == DONE y el otro statud 200 de codigo http
    // La respuesta, aunque sea JSON, viene en formato texto, por lo que tendremos que hace run parse
    //console.log(JSON.parse(this.responseText));
    //console.log(xhttp.responseText);
  }
};

// Endpoint de la API y método que se va a usar para llamar
xhttp.open("GET", "https://pokeapi.co/api/v2/pokemon/ditto", true);
//xhttp.setRequestHeader("Content-type", "application/json");
// Si quisieramos mandar parámetros a nuestra API, podríamos hacerlo desde el método send()
xhttp.send(null);

console.log(fetch("https://pokeapi.co/api/v2/pokemon/ditto"))

```

# Promesas

Para manejar promesas se tienen dos estados basicos por asi decirlo el 

> then: usado para indicar qué hacer en caso que la promesa se haya ejecutado con éxito.
> catch: usado para indicar qué hacer en caso que durante la ejecución de la operación se ha producido un error.

Las promesas regresan datos en un periodo de tiempo. Ambos métodos debemos usarlos pasándoles la función callback a ejecutar en cada una de esas posibilidades

Ejemplo

 ```
 referenciaFirebase.set(data)
  .then(function(){
    console.log('el dato se ha escrito correctamente');
  })
  .catch(function(err) {
    console.log('hemos detectado un error', err');
  });
  
  ```
  
  Para entender mejor el codigo esta encadenado y el codigo de arriba solo es para darle mejor visibilidad pero seria lo mismo sin espacios como aqui
  NOTA: No es nesario pasar ninguno de los dos y la promesa funcionaria
  
  ```
  referenciaFirebase.set(data).then(function(){console.log('el dato se ha escrito correctamente');}).catch(function(err) {console.log('hemos detectado un error',err')});

```
  
 ## Retorno de datos
 
 En el caso de las promesas cuanod se va a leer usalmente cuanod se escribe no pero podria ser las promesas regresan datos
 > En el caso que la promesa te devuelva un dato, lo podrás recibir como parámetro en la función callback que estás adjuntando al then().

```
funcionQueDevuelvePromesa()
  .then( function(datoProcesado){
    //hacer algo con el datoProcesado
  })
  ```
  
  ## Crear nuestras popias promesas
  
  Para crear nuestras promesas que van a hacer algo y regresar exito o fracaso se regresa el objeto promise comoen el siguiente ejemplo:
  
  ```
  function hacerAlgoPromesa() {
  return new Promise( function(resolve, reject){
    console.log('hacer algo que ocupa un tiempo...');
    setTimeout(resolve, 1000);
  })
}
```
Ese codigo es lo mismo que este:

```
function hacerAlgoPromesa(tarea) {
  function haciendoalgo(resolve, reject) {
    console.log('hacer algo que ocupa un tiempo...');
    setTimeout(resolve, 1000);
  }
  return new Promise( haciendoalgo );
}
```

Se ejecutaria asi:

```

hacerAlgoPromesa()
  .then( function() {
    console.log('la promesa terminó.');
  })
  
  ```
  Ejemplo de promesa ES6:
  
```
   fetch('https://pokeapi.co/api/v2/pokemon/ditto')
  .then( response => response.text() )
  .then( resultText => console.log(resultText) )
  .catch( err => console.log(err) );

```  

Mismo codigo pero siin arrow funcions

```
   fetch('https://pokeapi1.co/api/v2/pokemon/ditto')
  .then( function res (response) { return response.text(); })
  .then( function rest (resultText) {console.log(resultText)})
  .catch( function (err) {console.log("Hubo un error dude");console.log(err)});
  
```  


  > https://desarrolloweb.com/articulos/introduccion-promesas-es6.html


# Fetch ( mas nuevo que XMLHttpRequest())

> Su uso más simple consiste en pasarle una URL, cuyo contenido se traerá el cliente web de manera asíncrona.
fetch basa su trabajo en promesas ES6, por lo que nos devolverá una promesa que podemos tratar tal como estamos acostumbrados a hacer, con el "then" y el "catch".

Sintaxis basica con codigo completo

```


fetch('test.txt')
  .then(ajaxPositive)
  .catch(showError);
  
  function showError(err) { 
  console.log('muestor error', err);
}

function ajaxPositive(response) {
  console.log('response.ok: ', response.ok); // la propiedad "ok" de la respuesta nos ofrece información sobre si la solicitud produjo una respuesta con un código //positivo (un status 200 o similar) en el protocolo HTTP. La propiedad "status" nos ofrece el código de respuesta del servidor (200, 404, 500, etc.).
  if(response.ok) {
    response.text().then(showResult); // Accediendo mediante el metodo text()
  } else {
    showError('status code: ' + response.status);
    return false;
  }
}

function showResult(txt) {
  console.log('muestro respuesta: ', txt);
}

```

Para el codigo completo:

```

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Fetch</title>
</head>
<body>
  <button id="btn">Hacer una conexión con Ajax</button>
  
  <script>
  document.addEventListener('DOMContentLoaded', configureAjaxCalls);

  function configureAjaxCalls() {
    document.getElementById('btn').addEventListener('click', function() {
      fetch('test.txt')
        .then(ajaxPositive)
        .catch(showError);
    });

    function ajaxPositive(response) {
      console.log('response.ok: ', response.ok);
      if(response.ok) {
        response.text().then(showResult);
      } else {
        showError('status code: ' + response.status);
      }
    }

    function showResult(txt) {
      console.log('muestro respuesta: ', txt);
    }

    function showError(err) { 
      console.log('muestor error', err);
    }
  }
  </script>
</body>
</html>
```



## Acceder al texto de la respuesta de Ajax con fetch

Para acceder a la respuesta se usa la el metodo text 
>El método que nos devuelve el texto del servidor se llama text(). Es un método existente en el objeto de la respuesta recibida. Sin embargo, acceder al contenido de la respuesta, el texto que el servidor nos envía, no es tan trivial. El motivo es que el método text() devuelve otra promesa, lo que nos obliga a tratarla de nuevo con el correspondiente then / catch.

Para accder a la informacion de una api necesitamos checar la respuesta despues s es positiva sacarla con otra pormesa del metodo teext() de la siguiente manera:

```
fetch('https://randomuser.me/api/?results=10')
  .then( response => {
    if(response.status == 200) {
      return response.text();
    } else {
      throw "Respuesta incorrecta del servidor" 
    }
  })
  .then( responseText => {
    let users = JSON.parse(responseText);
    console.log('Este es el objeto de usuarios', users);
  })
  .catch( err => {
    console.log(err);
  });
  
  ```
 
> https://desarrolloweb.com/articulos/fetch-ajax-javascript.html

# Document.location vs Window.location

> window.location y document.location: estos objetos se utilizan para obtener la URL (la dirección de la página actual o actual) y desviar el navegador a una nueva página o ventana. La principal diferencia entre ambos es su compatibilidad con los navegadores.

Entonces analisando el parrafo anterior es un objeto entonses si le hacemo un "console.log(windows.location/document.location)" regresara un objeto recordar que los
objetos en Java Script son como JSON.

```
console.log(window.location/document.location)// e imprime el objeto
Location {ancestorOrigins: DOMStringList, href: 'https://developer.mozilla.org/es/docs/Web/JavaScript/Guide/Working_with_Objects', origin: 'https://developer.mozilla.org', protocol: 'https:', host: 'developer.mozilla.org', …}

```

En la ciberseguridad he visto que se usan para redireccionar las paginas por ejemplo:

```
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8">
	<meta name="viewport" content="width=device-width, initial-scale=1">
	<title></title>
</head>
<body>

	<script type="text/javascript">
		
		document.location = "https://www.google.com.mx/";

		//window.location.href = "https://www.delftstack.com/howto/";

	</script>

</body>
</html>

```



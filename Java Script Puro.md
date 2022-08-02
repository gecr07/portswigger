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

## Promesas

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
  
  ```
  referenciaFirebase.set(data).then(function(){console.log('el dato se ha escrito correctamente');}).catch(function(err) {console.log('hemos detectado un error',err')});

```
  
 

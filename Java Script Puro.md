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

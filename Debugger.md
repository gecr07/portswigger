
# Source Maps

>Basically it's a way to map a combined/minified file back to an unbuilt state. When you build for production, along with minifying and combining your JavaScript files, you generate a source map which holds information about your original files. When you query a certain line and column number in your generated JavaScript you can do a lookup in the source map which returns the original location. Developer tools (currently WebKit nightly builds, Google Chrome, or Firefox 23+) can parse the source map automatically and make it appear as though you're running unminified and uncombined files.

> Uno de los problemas cuando se trata de código generado por máquina surge cuando los desarrolladores intentan depurar su código. Cualquier error arrojado por el navegador corresponderá al archivo JS generado, no al código fuente original de React. Entonces, si el navegador dice que hay un error en la línea 300, el desarrollador no tiene forma de hacer corresponder ese número de línea con el código React original. Para combatir esto, se introdujeron mapas fuente y se pueden usar para vincular de nuevo al código fuente original. Si se encuentra un mapa fuente, Google Chrome compilará automáticamente el código fuente original como se muestra a continuación:





# Transpiladores (ReacJS) Atacando ReactJS

> Los transpilers, o compiladores fuente a fuente, son herramientas que leen el código fuente escrito en un lenguaje de programación y producen el código equivalente en otro lenguaje. React JS es un lenguaje de programación; pero aunque los navegadores no entienden React, sí entienden JavaScript. Esto significa que para que el navegador entienda el código React JS, primero se debe usar un transpilador para convertir el código React en código JavaScript nativo.

> https://redsentry.com/javascript-source-maps/


Osea se busca ver si esta pagina usa el mapeo de archivos para poder ver el codigo fuente original en busca de:

-Claves API codificadas

-Contraseñas codificadas

-Defectos lógicos

-XSS

# Referencias

> https://getmantra.com/web-app-security-testing-with-browsers/
> https://www.youtube.com/watch?v=Y1S5s3FmFsI
> 

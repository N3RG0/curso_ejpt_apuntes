# Curso Pentester Junior
Un curso que abarca la gran mayoria de conocimientos necesarios para aprobar la certificacion eJPT.

# Metodologias de Recopilacion de Informacion.
La recopilación de información es la primera fase de cualquier prueba de penetración, y consiste esencialmente
en recopilar informacion sobre un objetivo ya sea persona, sitio web, empresa o sistema objetivo. Mientras mas 
conozcas tu objetivo mejor podrás desenbolverte en las proxímas etapas, como enumeracion, explotacion, etc. Recordando
que esas etapas dependen de la información que has logrado obtener en esta primera fase, la recopilacion de informacion se 
divide en dor tipos. Tienes la RECOPILACIÓN PASIVA que trata sobre obtener la mayor informacion posible de fuentes públicas,
como su direccion IP real, tecnologias web etc. Y la RECOPILACIÓN ACTIVA aqui es donde ya nos damos a conocer a nuestro
objetivo ya que interactuamos directamente con él, en esta fase podremos encontrar puertos abiertos, conocer la infraestructura
interna del nuestro objetivo entre otros (Realizar una recopilación activa de información es ilegal, y no debería realizarse 
sin consentimiento del propietario del objetivo).

## Recopilacion pasiva de informacion
### Reconocimiento y Huella digital de un sitio web
Vamos a explorar la recopilacion pasiva de informacion en un sitio web, tambien exploraremos un pooco la huella digital
pero. Que es la huella digital? la huella digital es esencialmente lo mismo que el reconocimiento pero la diferencia es que 
estamos identificando informacion mas importante que es pertinente a un objetivo en particular. 
El primer bit de informacion que vamos a buscar es la direccion IP del sitio web real o del servidor web que aloja el sitio,
cualquier directorio que pueda estar oculto a los motores de busqueda, cualquier nombre en el sitio web, direcciones de correo,
numeros telefonicos, cualquier cosa que nos pueda dar una mejor compresion de nuestro objetivo, y por su puesto en el caso de un
sitio web la tecnologia web que esta siendo utilizada entonces podemos comenzar con esto: 

#### Obtener la direccion IP de un sitio Web

En algunos casos o en la mayoria, cuando se trata de un entorno de produccion, el sitio puede estar detras de un proxy o un firewall,
como CludFlare u otros, entonces para obtener la IP de un dominio simplemente abrimos nuestra terminal (Estare usando Kali Linux) y 
utilizamos el comando *host*, por ejemplo:
~~~
host https://mipaginaobjetivo.com
~~~
Esto nos devolvera la direccion IPV4 del objetivo, la direccion IPV6 en caso de que el dominio lo posea, como tambien los servidores de correo. 
Si nos encontramos con mas de una direccion IPV4 lo mas probable es que el sitio se encuentre protejido por un proxy o firewall. 

#### Archivo robots.txt 
Hasta ahora logramos obtener la direcion IP de la pagina web objetivo, otra verificacion que podemos realizar con respecto a la informacion de 
identificacion seria buscar enlaces a redes sociales o cualquier informacion que nos permita conocer mejor nuestro objetivo. Sin embargo cuando 
hablamos de explorar un sitio web o de buscar informacion en un sitio web el mejor lugar para comenzar es el archivo *robots.txt*, pero... 
como accedemos a el? Pues muy sencillo luego del nombre de dominio agregamos **/robots.txt**:
~~~
https://mipaginaobjetivo.com/robots.txt
~~~
Este es un archivo de texto que contiene algunas entradas, vamos con la explicacion. Cuando tienes un sitio web es muy comun que un motor 
de busqueda como Google o Bing rastreara el sitio web, en que sentido? pues esencialmente recorre el sitio web y luego lo indexa en 
google.com, etc. Para que cuando alguien busque por ejemplo "Aprender hacking gratis", ese sitio web es mostrado y cuando pasa esto 
las motores de busqueda podrian potencialmente estar revelando informacion que no desea que se haga publica, y es aqui donde entra en juego
este archivo que tendria un formato como:
~~~
User-agent: *
Disallow: /wp-admin/
Allow: /cart
Disallow: /orders
~~~

Casi todos los sitios web lo tienen, si un sitio web no lo tiene entonces realmente esta haciendo algo mal, porque esencialmente le permite
especificar qeu carpetas o archivos no desea que los motores de busqueda indexen. Entonces en este ejemplo podemos ver que especifica
un rechazo en *Disallow: /orders* y *Disallow: /wp-admin/* , pero bien NERGO que significa eso? simple significa que les informamos a los 
motores de busqueda cada vez que buscas el sitio web ignore esos directorios o archivos. y por que no quedria que los motores de busqueda lo 
indexen, bueno, eso es por que esos son directorios restringidos. En terminos generales es una buena practica de seguridad evitar que esos 
directorios sean indexados por un motor de busqueda. en el ejemplo solo con mirar podemos saber que el sitio ejecuta wordpress ya que posee 
el distintivo **/wp-*/**.

#### Archivo sitemap.xml
Que es un mapa del sitio? un mapa del sitio es esencialmente un archivo para proporcionar a los motores de busqueda una forma realmente
organizada de indexar los sitios web, y como accedemos a el? al igual que con el robots.txt solo agregaremos */sitemap_index.xml* de la
siguiente manera:
~~~
https://mipaginaobjetivo.com/sitemap_index.xml
~~~
Este archivo nos permitira conocer mejor a nuestro objeticvo ya que contiene un lista de paginas que pueden ser de acceso publico o que 
puede ser indexado por un motor de busqueda. Esto resulta util ya que si una pagina en particular no estaba vinculada en el sitio principal
o en el FrontEnd podria indicarnos o darnos una idea de algunos de los otros enlaces o algunos de las demas paginas que se pueden acceder
publicamente.

#### Complementos de Navegador
UN complemento (addon, extension) muy util es **buitlwhith**. Este es un perfilador de tecnologias web que esencialmente te dira que tecnologias
esta ejecutando en el sitio web.

#### Utilidad whatweb



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

#### Archibo robots.txt 
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
este archivo.


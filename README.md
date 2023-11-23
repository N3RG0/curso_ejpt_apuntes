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

El comando whatweb viene preempaquetado en kali linux, es tambien un perfilador de tecnologias web, que basicamente nos ayudara a conocer informacion
importante sobre un sitio web en  particular para usarlo de forma basica podemos usar *whatweb <ip o dominio>* por ejemplo:

~~~
watweb https://mipaginaobjetivo.com
~~~
lo que nos proporcionara mucha informacion de la siguinte manera:
~~~
http://mipaginaobjetivo.com [302 Found] Apache[2.4.18], HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)],
IP[ip dela pagina], RedirectLocation[https://www.mipaginaobjetivo.com]https://www.mipaginaobjetivo.com [200 OK] Apache[2.4.18], Bootstrap, Cookies[PHPSESSID],
HTML5, HTTPServer[Ubuntu Linux][Apache/2.4.18 (Ubuntu)], IP[ip de la pagina], JQuery, Script[text/javascript], Title[El titulo real de la pagina], X-UA-Compatible[IE=edge]
~~~

podemos observar tanto la ip real como las tecnologias que se estan ejecutando en dicho servidor, en este caso podemos observar que el servidor es un servidor
Ubuntu y la version de Apache es la 2.4.18, y se usan tecnologias como JQuery, Bootstrapc etc. informacion que nos permite conocer mejor a nuestro objetivo. 

### WHOIS obtener datos acerca de un determinado dominio
Whois es un protocolo, aqui podemos encontrar informacion a cerca del registro de un determinado dominio como su fecha de registro, renovacion y caducidad.
Tambien podemos encontrar datos a cerca del propietario pero la mayoria de veces esta informacion se encuentra redactada o protegida esto quiere decir que
la informacion que nos muestra en la mayoria de los casos no es la informacion real del propietario de dicho nombre de dominio, en Kali tenemos el comando *whois*
y podemos usarlo de la siguiente manera:
~~~
whois mipaginaobjetivo.com
~~~
esto nos devolvera mucha informacion acerca del registro del dominio masomenos algo como esto:
~~~
   Domain Name: GOOGLE.COM
   Registry Domain ID: 2138514_DOMAIN_COM-VRSN
   Registrar WHOIS Server: whois.markmonitor.com
   Registrar URL: http://www.markmonitor.com
   Updated Date: 2019-09-09T15:39:04Z
   Creation Date: 1997-09-15T04:00:00Z
   Registry Expiry Date: 2028-09-14T04:00:00Z
   Registrar: MarkMonitor Inc.
   Registrar IANA ID: 292
   Registrar Abuse Contact Email: abusecomplaints@markmonitor.com
   Registrar Abuse Contact Phone: +1.2086851750
   Domain Status: clientDeleteProhibited https://icann.org/epp#clientDeleteProhibited
   Domain Status: clientTransferProhibited https://icann.org/epp#clientTransferProhibited
   Domain Status: clientUpdateProhibited https://icann.org/epp#clientUpdateProhibited
   Domain Status: serverDeleteProhibited https://icann.org/epp#serverDeleteProhibited
   Domain Status: serverTransferProhibited https://icann.org/epp#serverTransferProhibited
   Domain Status: serverUpdateProhibited https://icann.org/epp#serverUpdateProhibited
   Name Server: NS1.GOOGLE.COM
   Name Server: NS2.GOOGLE.COM
   Name Server: NS3.GOOGLE.COM
   Name Server: NS4.GOOGLE.COM
   DNSSEC: unsigned
   URL of the ICANN Whois Inaccuracy Complaint Form: https://www.icann.org/wicf/
etc....
~~~
como podemos observar obtenemos la fecha de creacion de dicho dominio en este caso google.com fue creado en 1997,
y fue actualizado en 2019, obtenemos el ID de registro, la fecha de expiracion en este caso 2028, y el registrador
que es MarkMonitor Inc. En algunos casos nos mostrara datos del propietario pero como anteriormente se mensiono la
mayoria de veces estos datos no son los reales, esto nos servira para conocer mucho mas a nuestro objetivo.
Tambien podemos usar la herramienta en linea: https://who.is/. La cual nos facilitara la comprension de los  datos
antes obtenidos clasificandolos por categoria y sean mucho mas faciles de consumir.

#### Reconocimiento de DNS
Mediante el reconocimiento de DNS estaremos tratando de identificar los registros asociados con un domino en particular
como talvez un servidor de correo, direcciones ip, registros txt, cualquier cosa que pueda proporcionarnos una mejor idea
de como nuestro objetico esta configurado para funcionar, seguimos en la recopilacion pasiva de informacion asi, en Kali
linux tenemos la herramienta *dnsrecon* y una manera basica de usarla es:
~~~
dnsrecon -d mipaginaobjetivo.com
~~~
lo cual nos dara una salida similar a esta:
~~~
[*] std: Performing General Enumeration against: zonetransfer.me...
[-] All nameservers failed to answer the DNSSEC query for zonetransfer.me
[*]      SOA nsztm1.digi.ninja 81.4.108.41
[*]      NS nsztm1.digi.ninja 81.4.108.41
[*]      Bind Version for 81.4.108.41 secret"
[*]      NS nsztm2.digi.ninja 34.225.33.2
[*]      Bind Version for 34.225.33.2 you"
[*]      MX ALT1.ASPMX.L.GOOGLE.COM 172.217.197.26
[*]      MX ALT2.ASPMX.L.GOOGLE.COM 108.177.12.27
[*]      MX ASPMX4.GOOGLEMAIL.COM 172.253.62.26
[*]      MX ASPMX3.GOOGLEMAIL.COM 108.177.12.27
[*]      MX ASPMX.L.GOOGLE.COM 74.125.138.27
[*]      MX ASPMX5.GOOGLEMAIL.COM 64.233.186.27
[*]      MX ASPMX2.GOOGLEMAIL.COM 172.217.197.27
[*]      MX ALT1.ASPMX.L.GOOGLE.COM 2607:f8b0:400d:c0f::1a
[*]      MX ALT2.ASPMX.L.GOOGLE.COM 2607:f8b0:400c:c08::1a
[*]      MX ASPMX4.GOOGLEMAIL.COM 2607:f8b0:4004:c07::1a
[*]      MX ASPMX3.GOOGLEMAIL.COM 2607:f8b0:400c:c08::1a
[*]      MX ASPMX.L.GOOGLE.COM 2607:f8b0:4002:c0c::1b
[*]      MX ASPMX5.GOOGLEMAIL.COM 2800:3f0:4003:c00::1b
[*]      MX ASPMX2.GOOGLEMAIL.COM 2607:f8b0:400d:c0f::1b
[*]      A zonetransfer.me 5.196.105.14
[*]      TXT zonetransfer.me google-site-verification=tyP28J7JAUHA9fw2sHXMgcCC0I6XBmmoVi04VlMewxA
[*] Enumerating SRV Records
[+]      SRV _sip._tcp.zonetransfer.me www.zonetransfer.me 5.196.105.14 5060
[+] 1 Records Found
~~~
Podemos observar varios registros MX que se refieren a los servidores de correo, en este caso se trata de 
los servidores de correo de Google o G-suite, tambien tenemos el txt de la verificacion de sitio de Google
tenemos varias direcciones ip pero no entraremos en detalle ya que existe una herramienta web como la anterior
que nos facilitara la comprension de toda esta informacion esa herramienta es: https://dnsdumpster.com/ 
solo tenemos que proporcionar en el cuadro de busqueda el dominio objetivo y nos dara detalles acerca de subdominios,
servidores de correo, dns, y hosts. Tambien nos facilitara una imagen para comprender mejor la jerarquia de dicho objetivo
y herramientas adicionales como el escaneo de puertos (el escaneo de puertos es recopilacion activa de informacion asi que 
debemos contar con el permiso del propietario para llevarla a cabo). 

#### Verificar WAF (Firewall de aplicaciones Web)
Bien tenemos muchos datos y conocemos cada vez mejor a nuestro objetivo, algo que deseariamos saber es si dicho objetivo
se encuentra detras de un firewall o proxy, es sumamente sencillo en nuestro Kali tenemos la herramienta llamada *wafw00f*.
que nos indicara si un determinado dominio se encuentra detras de un WAF, y la usaremos de la siguiente manera:

~~~
wafw00f mipaginaobjetivo.com -a
~~~
y nos debolvera o bien:
~~~
[*] Checking https://mipaginaobjetivo.net
[+] Generic Detection results:
[-] No WAF detected by the generic detection
[~] Number of requests: 7
~~~
lo que significaria que en este domino no se encontro un firewall, tambien podemos obtener algo como:
~~~
[*] Checking https://mipaginaobjetivo2.com
[+] The site https://mipaginaobjetivo2.com is behind Cloudflare (Cloudflare Inc.) WAF.
[~] Number of requests: 2
~~~
genial ahora nos dice que el dominio mipaginaobjetivo2.com, se encuentra bajo un firewall de Cloudflare
entonces la IP que podemos obtener mediante el uso del comando *host* no es especificamente la IP del servidor
que aloja la pagina web objetivo. un dato interesante los WAF no protegen la direccion IP real de los servidores 
de correo (las etiquetas MX que encontramos anteriormente en dnsrecon).


                                 

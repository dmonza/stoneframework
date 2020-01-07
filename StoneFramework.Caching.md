# StoneFramework.Caching

Esta documentación está en revisión ya que GeneXus 15 incoporó varios motores de caching built-in.

## Que es y porque debe gestionar Cache en sus aplicaciones

La gestión de cache es prácticamente escencial en las aplicaciones modernas, si queremos que éstas sean rápidas.

Básicamente los que hace el cache es, cuando el usuario solicita información de la base de datos, la primera vez obtenida de la base de datos, se carga a memoria y se le retorna al usuario. La siguientes veces que se consulte la misma información y si la misma no tuvo cambios, se traerá de memoria.
De ésta forma que le da úna respuesta más rápida al usuario y se evita sobrecargar el motor de base de datos por consultas innecesarias.

## Como implementa el cache Stone Framework

Se dispone de dos procedimientos que se abraen de la forma de realizar el cache a fín de controlar:

1. Timeout: Tiempo de espera antes de leer nuevamente la base de datos
2. Encriptación: Muchos de los motores de cache (como memcached) no incorporan encriptación porque no es su trabajo y deben ser performantes. El problema es que a veces se cachea infromación sencible y un hacker podría eventualmetne consultarla.

### SGP_CacheSet

Crea un objeto de cache (SDT:SGP_CacheType). Gestiona encriptación y time out.

Sintaxis: SGP_CacheSet( Character-in:&Key, Character-in:&Value, Numeric-in:&TimeoutSeconds, Bool-in:&IsEncripted, SGP_CacheType-out:&SGP_CacheType)

&Key : Clave con la que se guardará el objeto en cache
&Value: Valor a ser almacenado
&TimeoutSeconds: Time out para el cache. Si es 0, se toma el timeout por defecto definido en StoneFrameworkConfig

&IsEncripted: Se debe encriptar el valor?

Retorno:
&SGP_CacheType : SDT para luego se cacheado (Memcached, websession, etc.) mediante ToXml/ToJson

### SGP_CacheGet

Obtiene el valor de cache desde un objeto de cache (SDT:SGP_CacheType)

Sintaxis: SGP_CacheGet( Character-in:&Key, Bool-in:&IsEncripted, SGP_CacheType-in:&SGP_CacheType, Character-out:&Value )

&Key : Clave con la que se guardó el objeto en cache
&IsEncripted: El valor almacenado está encriptado?
&SGP_CacheType : SDT obtenido desde cache mediante FromXml/FromJson

Retorno:
&Value: Valor que fué almacenado

## Que motores de cache puedo utilizar
Primero hay que tener en cuenta que existem muchas formas de almacenar información en cache y que este puede ser centralizado y des-centralizado.

Es lógico que si se leen datos de la DB que pueden ser consultados por diferentes usuarios, se utilice un cache centralizado, de forma de optimizar la gestión de este cache (ej.memcached). Por el contrario, si es información sensible para un usuario en particular, se podría crear un cache solo para el usuario, por ejemplo en la websession.

Hay que tener en cuenta que otros factores que pueden afectar estas deciciones incluyendo la aplicación que se está diseñando y su arquitectura. Entre estos factores podría estar la decición de realizar balance de carga en el que todos los servidores de la granja debieran apuntar a un solo servidor de cache si se desea centralizar y optimizar la aplicación.

Websession (disponible solo en web)
No es muy estandar pero es una opción y está impelmentado en StoneFramework. Hay que tener en cuenta que este cache solo es valido para cada usuario, por lo que no es centralizado y solo el usuario puede tener acceso a la información guardada; ésto quiere decir que el resto de los usaurios no podrá acceder al cache de otro usuario.

### CachingWebsessionSet

Guarda datos en cache WebSession.

Sintaxis: CachingWebsessionSet( Character-in:&Key, Character-in:&Value, Numeric-in:&Timeout, Bool-in:&IsEncripted)

&Key : Clave con la que se guardará el objeto en cache
&Value: Valor a ser almacenado
&Timeout: Timeout en segundos
&IsEncripted: Se debe encriptar el valor?

### CachingWebsessionGet

Obtiene cache desde WebSession.

Sintaxis: CachingWebsessionGet( Character-in:&Key, Bool-in:&IsEncripted, Character-out:&Value )

&Key : Clave con la que está almacenado el objeto en cache
&IsEncripted: El valor está encriptado?

Retorno:
&Value: Valor que fue almacenado

## Memcached
Memcached es uno de los estandar en lo que a cache se refiere y es un servidor de cache centralizado. Está implementado como servidor perse, también mysql ofrece una extensión para que la DB se transforme en un servidor de Memcached y Amazon ofrece su servicio entre otros varios proveedores.

En nuestro caso, utilizamos particularmente un motor simple para windows que se puede descargar de aquí

Por más información de Memcached:
https://en.wikipedia.org/wiki/Memcached
http://memcached.org/

Extensión para Memcached

GProjex tiene publicada en Genexus Marketplace un extension para trabajar  con Memcached la cual puede ser consultada en:
https://marketplace.genexus.com/viewproduct.aspx?598

gpxMemcachedSet

Guarda datos en Memcached.

Se debe definir una variable de entorno llamada "MEMCACHED_HOST" donde se especificará la IP/Host del servidor de Memcached.

Sintaxis: gpxMemcachedSet( Character-in:&Key, Character-in:&Value, Numeric-in:&Timeout, Bool-in:&IsEncripted)

&Key : Clave con la que se guardará el objeto en cache
&Value: Valor a ser almacenado
&Timeout: Timeout en segundos
&IsEncripted: Se debe encriptar el valor?

gpxMemcachedGet

Obtiene cache desde WebSession.

Sintaxis: gpxMemcachedGet( Character-in:&Key, Bool-in:&IsEncripted, Character-out:&Value )

&Key : Clave con la que está almacenado el objeto en cache
&IsEncripted: El valor está encriptado?

Retorno:
&Value: Valor que fue almacenado

¿Pero como incorporo esto a mis aplicaciones?
Es muy sencillo, como dijimos, se debe leer la información de la base de datos y luego almacenarla en cache mientras no cambie.

Por ejemplo, si tuvieramos un procedimiento que nos retorna un SDT de un cliente, podríamos utilizar este código:

parm( in:CustomerID, in:&Force, out:&Customer);

&Key = &Pgmname+CustomerID.ToString().Trim()
&Value = gpxMemcachedGet(&Key, false)

// Si no se obtuvo desde cache o es una carga forzada, se lee desde DB
if &Value.IsEmpty() or &IsForce
   &Customer = new()

   for each
      &Customer.Id = CustomerID
      &Customer.Name = CustomerName
   endfor

   gpxMemcachedSet( &Key, false, &Customer.ToXml() )
else
   // Se carga el SDT con el cache obtenido
   &Customer.FromXml(&Value)
endif
Entonces, siempre que necesitamos obtener los datos del cliente, llamamos a este procedimiento con &IsForce = false.
Si modificamos el cliente, agergaríamos una regla para que llame a este procedimiento "on aftercomplete" con &IsForce = true para que refresque el cache.

Este procedimiento es análogo para implementar con WebSession caching.
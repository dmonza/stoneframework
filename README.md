# Stone Framework - Framework de desarrollo para GeneXus

Stone Framework Versión: 2.0

## Tabla de Contenidos

  1. [Qué es Stone Framework](#Qué-es-Stone-Framework)
  1. [Primeros Pasos](#primeros-pasos)
  1. [Configuración](#configuración)
  1. [Módulos](#módulos)
  1. [Patrones de diseño](#patrones-de-diseño)
  1. [Tipos de datos](#tipos-de-datos)
  1. [Generación de Logs](#generación-de-logs)
  1. [Licencia](#licencia)

## Qué es Stone Framework
Toda construcción comienza por la piedra fundamental. Aquí es donde Stone Framework tiene un papel importante ya que nos ayuda avanzar rápidamente en nuestros proyectos Genexus.

Se trata de un conjunto de funcionalidades y patrones de diseño que son utilizados en muchas de nuestras KBs. Es muy útil para iniciar KBs de cero o agregar funcionalidades a las KBs existentes.

Stone Framework está desarrollado mediante un [estándar de desarrollo](https://github.com/sincrum/genexus) en el cual se mantiene la homogeneidad en todo su código. Ésto logra minimizar la curva de aprendizaje para el programador que lo utilice.

Es un framework desarrollado por Daniel Monza originalmente para [GProjex](http://www.gprojex.com/) y comercializado por [Sincrum](http://www.sincrum.com/).

**[Volver al inicio](#tabla-de-contenidos)**

## Primeros pasos

Para comenzar a utilizar las funcionalidades del Stone Framework se deberan seguir los siguientes pasos:

1. Incorporar Stone Framework a la KB GeneXus.
1. Si se está generando en C#, se deberá agregar a la propiedad **Compiler Flags** del generador **/r:bin\log4net.dll**. Por más información sobre este punto, revisar SGP_System_LogAdd.
1. En la raiz de la KB, crear el procedimiento **StoneFrameworkConfig** basado en **StoneFrameworkConfigExample**.

**[Volver al inicio](#tabla-de-contenidos)**

## Configuración

En ciertas aplicaciones o ambientes de ejecución, algunos parámetros del framework, deberán ser configurados de forma particular.

Como se vio en [Primeros Pasos](#primeros-pasos), tanto en Stone Framework como en los patrones de diseño, se deben crear procedimientos de configuración en la raiz de la KB GeneXus basados en los que tierminan con el texto "Example". Esto evita sobre-escribirlos al consolidar una nueva versión del framework.

### StoneFrameworkConfig

Es conveniente que esté fuera de la estructura de carpetas de StoneFramework ya que va a tener las configuraciónes particulares para cada KB.

Items de configuración
General

Ruta de archivos temporales.

&SGP_ConfigType.TempFilesPath = !"..\PrivateTempStorage\"

Files pattern

Ruta de archivos Públicos.

&SGP_ConfigType.Files.PublicFilesPath = !"PublicFiles\"

Ruta de archivos Privados.
&SGP_ConfigType.Files.PrivateFilesPath = !"..\PrivateFiles\"

Time out por defecto de validez para links de archivos privados
&SGP_ConfigType.Files.PrivateFilesTimeout = 300

Serials Pattern

Largo de series hexadecimales
&SGP_ConfigType.Serials.HexLength = 8

Caching

Clave de encriptación. Utilizada cuando se desea encriptar el cache; ésto útlimo se decide cuando se llama a los proc. de cache, ej: CachingWebSessionSet.
&SGP_ConfigType.Cache.EncryptionKey = !"C1F55DD20AE05D9ADF4988C4ADF70A8D"

Time out por defecto
&SGP_ConfigType.Cache.Timeout = 600

Mail Settings

Dentro de "&SGP_ConfigType.Mail" se encuentran todos los parámetros configurables para envío de mail.

Revisar **EmailSendBase**


**[Volver al inicio](#tabla-de-contenidos)**

## Módulos

1. [StoneFramework.Business](StoneFramework.Business.md)
Funcionalidades relacionadas con el negocio
1. [StoneFramework.Caching](StoneFramework.Caching.md)
Gestión de cache.
1. [StoneFramework.Net](StoneFramework.Net.md)
Funcionalidades de Red
1. [StoneFramework.System](StoneFramework.System.md)
Funcionalidades del sistema
1. [StoneFramework.Tools](StoneFramework.Tools.md)
Herramientas varias
1. [StoneFramework.TestUnit](StoneFramework.TestUnit.md)
Testeo unitario.

**[Volver al inicio](#tabla-de-contenidos)**

## Patrones de diseño

**[Volver al inicio](#tabla-de-contenidos)**

## Tipos de datos

### Structured Data Types

- CommonResponse
	- ErrCode
	- ErrDescription
	- Messages
	- Id
	- Data

### Dominios

- Amount: Importes (-15.2)
- DateFull: Date with four year digits
- Json: Char(10M)
- Names: Char(64)
- Path: Char(256)
- Percentage: Porcentaje (6.2)
- Rates: Numeric(14.4)
- Token: Char(64)

### Dominios enumerados

- IdentificationTypes: Tipos de documentos de identidad
- Generators: Generadores GX. Ej. C#/Java
- HTTPMethods: HTTP request methods
- Languages: Lenguajes que pueden tener los usuarios
- LogLevels: Niveles de Log (Debug, Warning, Error, etc.)
- SitemapFrequencies: Frecuencias de actualización en sitemaps

**[Volver al inicio](#tabla-de-contenidos)**

## Generación de Logs
Para generar los logs del sistema, StoneFramework utiliza la biblioteca log4net provista por Genexus.

Según apache:

*"Esta librería permite habilitar log en tiempo de ejecución sin modificar los binarios de la aplicación. La librería fue diseñada para que las sentencias de log puedan ser enviadas junto con la aplicación sin incurrir en problemas de performance"*.

Por otro lado se puede configurar log4net en tiempo de ejecución para que guarde el log en un archivo, lo envíe por mail, lo agregue al trace de .net entre lo que lo hace sensacional al momento que necesitamos manipular los “logs” en nuestra aplicación.

Próximamente, se espera que un colaborador especialista en Java, pueda extender la funcionalidad a éste lenguaje mediante log4j.

Esta librería clasifica los logs de 6 formas:

- OFF - Apagado
- Debug
- Info - Información
- Warn - Advertencias
- Error
- Fatal - Errores fatales

Es una clasificación jerárquica en la que estableciendo "debug" se loguea todo (info, warn, error, etc) y a medida que se va subiendo en la clasificación, se va limitando la generación de logs, por ejemplo, si se especifica "Error", se generan logs de "error y fatal".

### Configuración para generar logs en Genexus
Genexus ya generar los logs de la aplicación mediante esta librería al setear las propiedades del generador:

Log level = [el nivel de log que deseamos ej.: debug]

Luego se puede elegir donde se va a generar. Por defecto está File y cliente.log (el archivo que contendrá el log).

### Generación de nuestros Logs
Para generar logs, StoneFramework dispone de la función [LogAdd](https://github.com/sincrum/stoneframework/blob/master/StoneFramework.System.md#logadd) la cual envía logs a la librería log4net y log4j (próximamente).

Como se puede revisar en la sintaxis, los parámetros son:

- &LogLevels : Nivel de log (debug, warn, info, etc.)
- &LogGroup : Clasificación de log, podría ser el nombre de nuestra aplicación. Si es vacío se utiliza la clasificación "StoneFramework"
- &Program : Nombre de programa o identificado que generar el log. Es un dato informativo para encontrar el programa que generó el log. Generalmente &Pgmname.
- &LogDsc : Texto libre con información de log a generar.

### Filtrando la información de logs que se generan
Al especificarle a Genexus que se generen logs, se va a generar una gran cantidad de información y esto proporcionará una lentitud en el sistema.

Para filtrar la información que se genera, debemos modificar los archivos de configuración de la aplicación. En .net serían web.config o cliente.exe.config.

Si deseamos evitar que se generen los logs de Genexus, realizaremos el siguiente cambio:

```javascript
<root>
      <level value="DEBUG" /> ---> cambiar por ---> <level value="OFF" />
      <appender-ref ref="RollingFile" />
</root>
```

Si deseamos solo generar los logs de nuestro &LogGroup, se debe agregar un nuevo "Logger" al archivo de configuración, éste se puede agregar antes del elemento "<root>".

En el siguiente ejemplo, generaremos solo los logs de los llamados a la función SGP_System_LogAdd sin especificar &LogGroup (&LogGroup=""). Un ejemplo de esto sería:

```javascript
LogAdd.Call(LogLevels.DEBUG, "", &Pgmname, "Esta es la información de log a generar")
```

Las modificaciones al archivo de configuración para que se genere el log sería:

```javascript
<logger name="StoneFramework">
   <level value="DEBUG" />
</logger>
<root>
   <level value="OFF" />
   <appender-ref ref="RollingFile" />
</root>
```

Más información:

- [LogAdd](https://github.com/sincrum/stoneframework/blob/master/StoneFramework.System.md#logadd) Generar logs con StoneFramework
- https://logging.apache.org/log4net/
- http://logging.apache.org/log4j/1.2/
- http://wiki.genexus.com/commwiki/servlet/wiki?C%C3%B3mo+generar+trace+de+.Net+en+la+nube,
- http://wiki.genexus.com/commwiki/servlet/wiki?Log+output+property,

**[Volver al inicio](#tabla-de-contenidos)**

## Links de interes

- https://www.youtube.com/watch?v=nKhE2deCOWU
- http://www.genexus.com/noticias/leer-noticia/presentamos-stone-framework-by-gproject-en-genexus-open?es
- http://es.slideshare.net/genexus/rolling-the-stone-framework-daniel-monza

## Licencia

A partir de la versión 2.0, Stone Framework está licenciado de forma comercial según el documento: [Licencia de StoneFramework](LICENCIA.md)

**[Volver al inicio](#tabla-de-contenidos)**
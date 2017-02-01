# Stone Framework - Framework de desarrollo para GeneXus

Stone Framework Versión: 2.0

## Tabla de Contenidos

  1. [Qué es Stone Framework](#Qué-es-Stone-Framework)
  1. [Primeros Pasos](#primeros-pasos)
  1. [Configuración](#configuración)
  1. [Módulos](#módulos)
  1. [Patrones de diseño](#patrones-de-diseño)
  1. [Tipos de datos](#tipos-de-datos)
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

**[Volver al inicio](#tabla-de-contenidos)**

## Licencia

A partir de la versión 2.0, Stone Framework está licenciado de forma comercial según el documento: [Licencia de StoneFramework](LICENCIA.md)

**[Volver al inicio](#tabla-de-contenidos)**
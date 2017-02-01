# StoneFramework.System

## Tabla de Contenidos

1. [EnvVarGet](#envvarget)
1. [Generator](#generator)
1. [LogAdd](#logadd)
1. [NewGUID](#newguid)
1. [TempFileName](#tempfilename)
1. [WebPath](#webpath)

## EnvVarGet
Lenguajes: C#
Interfaces: Todas

Propósito
Obtiene el valor de una variable de ambiente del sistema operativo.

Sintaxis
EnvVarGet( Character-in:&Name, Character-out:&Value)

&Name          : Nombre de la variable a retornar.

Retorno:

&Value          : Valor de la variable a retornar.

Ejemplo
```javascript
&Valor = EnvVarGet( !"PATH")

// Resultado: C:\Windows;c:\....
```

**[Volver al inicio](#tabla-de-contenidos)**

## Generator
Lenguajes: C#, Java
Interfaces: Todos

Propósito
Obtiene en tiempo de ejecución el lenguaje en que fue generada la aplicación.

Ya que Genexus puede generar una aplicación en diferentes lenguajes, a veces se hace necesario saber sobre cual está corriendo nuestra aplicación a fin de tomar decisiones.

Por ejemplo, alguna configuración que utiliza nuestro sistema debe ser diferente par a C# que para Java.

Sintaxis
Generator(out:&Generator)

Retorno:

&Generator       : Variable basada en el dominio enumerado “Generators” donde se obtendrá el lenguaje en que fue generada la aplicación.

Ejemplo
```javascript
// Variable basada en el dominio enumerado “Generators”
&Generator = Generator()
```

**[Volver al inicio](#tabla-de-contenidos)**

## LogAdd
Lenguajes: C#
Interfaces: Todos

Propósito
Generación estándar de logs del sistema.

Genexus desde hace varias versiones incorpora en C# la librería log4net para generar Logs de la aplicación.

Log4net es un port de log4j (java), versión original creada por Apache.

Según apache:

"Esta librería permite habilitar log en tiempo de ejecución sin modificar los binarios de la aplicación. La librería fue diseñada para que las sentencias de log puedan ser enviadas junto con la aplicación sin incurrir en problemas de performance".

Por otro lado se puede configurar log4net en tiempo de ejecución para que guarde el log en un archivo, lo envíe por mail, lo agregue al trace de .net entre lo que lo hace sensacional al momento que necesitamos manipular los “logs” en nuestra aplicación.

Más información:
https://logging.apache.org/log4net/
http://logging.apache.org/log4j/1.2/

Notas:
En .net se debe agregar “/r:bin\log4net.dll” a “Compiler flags” (propiedad del generador)Se necesitaría colaboración para agregar compatibilidad con Java.

Sintaxis
LogAdd( LogLevels-in:&LogLevels, Character-in:&LogGroup, Character-in:&Program, Character-in:&LogText)

&LogLevels        : Niveles de log basado en el dominio enumerado LogLevels (Fatal, Error, Warn, Info, Debug)
&LogGroup        : Logger o grupo de logs. Particularmente Stone Framework realiza sus logs con el logger “StoneFramework”.
&Program           : Programa que está generando el log. Normalmente se pasa &Pgmname.
&LogText            : Texto a registrar.

Ejemplo
```javascript
LogAdd.Call( LogLevels.ERROR, "MyAppName", &Pgmname, “Usuario no autenticado” )
```

Nota: El ejemplo generará un log tipo error para el logger “MyAppName” con “Usuario no autenticado”.

**[Volver al inicio](#tabla-de-contenidos)**

## NewGUID
Lenguajes: Todos
Interfaces: Todos

Propósito
Obtiene en un string representando un GUID.

Ya en la GxEv2 se disponemos del tipo de datos GUID para generar estos. Este procedimiento viene de tiempo atrás pero es útil en algunos casos.

De todas formas, para generar el GUID se utiliza el nuevo tipo de datos.

Para más información:

http://wiki.genexus.com/commwiki/servlet/wiki?GUID

http://wiki.genexus.com/commwiki/servlet/wiki?GUID+data+type

Sintaxis
NewGUID ( Character-out:&GUIDStr)

Retorno:

&GUIDStr           : Variable de tipo Character representando un GUID.

Ejemplo
```javascript
&GUIDStr  = NewGUID ()

// Resultado: “b7e69de0-b17d-48ad-9fcb-7e678985a6e0”
```

**[Volver al inicio](#tabla-de-contenidos)**

## TempFileName
Lenguajes: Todos
Interfaces: Todos

Propósito
Genera la ruta completa a un archivo temporal.

Es muy útil para archivos que se generan desde el sistema, como ser: pdf, excel, etc.

Se utiliza la ruta para archivos temporales definida en “TempFilesPath” dentro de “StoneFrameworkConfig ”.

Sintaxis
TempFileName( Character-in:&FileName, Character-in:&Ext, Character-out:&FullName)

&FileName: Nombre del archivo a ser creado (sin extensión).
&Ext: Extensión del archivo

Retorno:

&FullName         : Nombre del archivo temporal.

Ejemplo
```javascript
&FullName = TempFileName ( “MiReporte”, “pdf”)

Resultado:

&FullName = “C:\wwwroot\myapp\web\..\PrivateTempStorage\1360_MiReporte.pdf”
```

Nota: La ruta de la aplicación variará según ésta.

**[Volver al inicio](#tabla-de-contenidos)**

## WebPath
Lenguajes: C#, Java
Interfaces: Web

Propósito
Obtiene la ruta donde está alojada la aplicación web que se está ejecutando.

Por ejemplo, si nuestro sitio web está publicado en:
C:\wwwroot\mysitioweb\web

La ruta anterior es la retornada por este procedimiento y es muy útil para manejo de archivos en web.

Sintaxis
WebPath( Character-out:&webpath)

Retorno:

&webpath          : Ruta a la carpeta donde se está ejecutando la aplicación.

Ejemplo
```javascript
&Path = WebPath()

Resultado: C:\wwwroot\mysitioweb\web\
```

**[Volver al inicio](#tabla-de-contenidos)**
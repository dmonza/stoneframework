# StoneFramework.Tools

## Tabla de Contenidos

1. [BaseChange](#basechange)
1. [*ComputeHash](#computehash)
1. [ContentTypeByExt](#contenttypebyext)
1. [DecToHex](#dectohex)
1. [FileExtensionGet](#fileextensionget)
1. [FormattedDateString](#formatteddatestring)
1. [HexToDec](#hextodec)
1. [HexToRGB](#hextorgb)
1. [ISO8601ToTime](#iso8601totime)
1. [MessageAdd](#messageadd)
1. [MessagesLog](#messageslog)
1. [MessagesShow](#messagesshow)
1. [OnlyNumbers](#onlynumbers)
1. [Repeat](#repeat)
1. [*TextToHtml](#texttohtml)
1. [TextTruncate](#texttruncate)
1. [TimeToISO8601](#timetoiso8601)
1. [ValidChars](#validchars)

## BaseChange
Lenguajes: Todos
Interfaces: Todos

Propósito
Nos permite realizar cambios de base numérica. La única que por el momento no se soporta es la unaria (base-1).

Este procedimiento es utilizado por los de conversión entre decimal y hexadecimal:
[DecToHex](#dectohex)
[HexToDec](#hextodec)

Por más información:
https://es.wikipedia.org/wiki/Sistema_de_numeraci%C3%B3n

Sintaxis
SGP_Tools_BaseChange(Character-in:&NumIn, Numeric-in:&BaseIn, Numeric-in:&BaseOut, Character-out:&NumOut)

&NumIn    : Número de entrada, especificado en la base &BaseIn
&BaseIn:  : Base de entrada
&BaseOut: Base a la cual se quiere convertir. Base de salida.

Retorno:

&NumOut            : Número convertido a la base de salida

Ejemplo
```javascript
&NumOut = SGP_Tools_BaseChange( “1F”, 16, 10) // &NumOut = "31"

&NumOut = SGP_Tools_BaseChange( “1F”, 16, 8) // &NumOut = "37"

&NumOut = SGP_Tools_BaseChange( “1F”, 16, 2) // &NumOut = "11111"
```

**[Volver al inicio](#tabla-de-contenidos)**

## ContentTypeByExt
Lenguajes: Todos
Interfaces: Todos

Propósito
Retorna el content-type relativo a una extensión de archivo.

Por más información se puede revisar:
http://en.wikipedia.org/wiki/Internet_media_type

Sintaxis
SGP_Tools_ContentTypeByExt( Character-in:&Extension, Character-out:&ContentType)

&Extension        : Extensión de archivo, ej.: “pdf”

Retorno:

&ContentType : Variable character conteniendo el content-type vinculado a la extensión.

Ejemplo
```javascript
SGP_Tools_ContentTypeByExt ( “pdf”, &ContentType)

Resultado:
&ContentType=”application/pdf”
```

**[Volver al inicio](#tabla-de-contenidos)**

## DecToHex
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un valor decimal a hexadecimal.

Por más información:

https://en.wikipedia.org/?title=Hexadecimal

Sintaxis
SGP_Tools_DecToHex( Numeric(18)-in:&Decimal, Character-out:&Hex)

&Decimal            : Numeric (18) – Valor en decimal.

Retorno:

&Hex    : Character – Valor hexadecimal.

Ejemplo
```javascript
&Hex = SGP_Tools_DecToHex(  3917 )

Resultado: 
&Decimal = "F4D"
```

**[Volver al inicio](#tabla-de-contenidos)**

## FileExtensionGet
Lenguajes: Todos
Interfaces: Todos

Propósito
Obtener la extensión de un archivo desde su nombre.

Sintaxis
SGP_Tools_FileExtensionGet ( Character-in:&FileNameTemp, Character-out: &Extension)

&FileNameTemp             : Character - Nombre de archivo.

Retorno:

&Extension        : Character - Extensión del archivo.

Ejemplo
```javascript
&Extension  = SGP_Tools_FileExtensionGet( “myarchivo.pdf”)

Resultado:
&Extension  = “pdf”
```

**[Volver al inicio](#tabla-de-contenidos)**

## FormattedDateString
Lenguajes: Todos
Interfaces: Todos

Propósito
Convertir un DateTime a Character con un determinado formato.

Sintaxis

SGP_Tools_FormattedDateString( DateTime-in:&DateTime, Character-in:&Picture, Character-out:&FormattedDate)

&DateTime: DateTime – Fecha/Hora a convertir.
&Picture: Character – Formato para convertir la fecha.

YYYY: Año en 4 dígitos
YY: Año en 2 dígitos
MMM: Nombre del mes
MM: Més en 2 dígitos
DDD: Nombre del día
DD: Día en 2 dígitos
hh: Hora en 2 dígitos
mm: Minutos en 2 dígitos
ss: Segundos en 2 dígitos
Retorno:

&FormattedDate            : Character – Texto con la fecha en el formato especificado.

Ejemplo
```javascript
// &DateTime contiene la fecha 21/12/79 19:34

&FormattedDate  = FormattedDateString ( “DDD, DD de MMM de YYYY”)

Resultado: &FormattedDate = “Viernes, 21 de Diciembre de 1979”
```

**[Volver al inicio](#tabla-de-contenidos)**

## HexToDec
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un valor hexadecimal en decimal.

Por más información:

https://en.wikipedia.org/?title=Hexadecimal

Sintaxis
SGP_Tools_HexToDec(Character-in:&Hex, Numeric(18)-out:&Decimal)

 

&Hex    : Character – Valor hexadecimal.

Retorno:

&Decimal            : Numeric (18) – Valor en decimal.

Ejemplo
```javascript
&Decimal = SGP_Tools_ HexToDec( “F4D”)

Resultado:
&Decimal = 3917
```

**[Volver al inicio](#tabla-de-contenidos)**

## HexToRGB
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un color hexadecima RGB.

Por más información:
http://wiki.genexus.com/commwiki/servlet/wiki?RGB+Function
https://es.wikipedia.org/wiki/RGB

Sintaxis
SGP_Tools_HexToRGB (Character-in:&HexColor, Numeric(8)-out:&RGBColor)

&HexColor         : Color en Hexadecimal. Debe especificarse en 6 caracteres.

Retorno:

&RGBColor         : Color RGB.

Ejemplo
```javascript
MyTextBlock.ForeColor = SGP_Tools_ HexToDec( “0000FF”)

Resultado:
Se coloreará de azul MyTextBlock.
```

**[Volver al inicio](#tabla-de-contenidos)**

## ISO8601ToTime
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un DateTime en texto con formato ISO8601.

Más información:
https://en.wikipedia.org/?title=ISO_8601

Sintaxis
SGP_Tools_ISO8601ToTime (Character-in:&iso, DateTime-out:&dt)

&iso: Fecha y hora en formato ISO8601

Retorno:

&dt: DateTime relativo a &ISO.

Ejemplo
```javascript
&dt  = SGP_Tools_ISO8601ToTime ( “2015-06-18T10:46:13”)

Resultado:
&dt contendrá la fecha hora: 18/06/2015 10:46:13
```

**[Volver al inicio](#tabla-de-contenidos)**

## MessageAdd
Lenguajes: Todos
Interfaces: Todos

Propósito
Agrega un texto a una colección de mensajes.

Sintaxis
SGP_Tools_MessageAdd( Messages-in:&Messages, in:&MsgStr)

&Messages        : Colección de mensajes

&MsgStr              : Texto del mensaje a agregar

Ejemplo
```javascript
MessageAdd (&Messages, “Contraseña no válida”)
```

**[Volver al inicio](#tabla-de-contenidos)**

## MessagesLog
Lenguajes: Todos
Interfaces: Todos

Propósito
Genera Log con toda una colección de mensajes.

Para profundizar en la generación de logs con StoneFramework, revisar el proc. SGP_System_LogAdd

Sintaxis
SGP_Tools_MessagesLog( Character-in:&Program, LogLevels-in:&LogLevels, Messages-in:&Messages)

&Program           : Programa que genera el log. Típicamente &Pgmname
&LogLevels        : Nivel de log (DEBUG, INFO, WARN, etc.)
&Messages        : Colección de mensajes a registrar

Ejemplo
Se utiliza por ejemplo en los casos donde no queremos que el usuario vea los mensajes generados por un Business Component, pero si lo queremos tener un registro para saber que sucedió.

En el ejemplo tenemos el BC Cliente:
```javascript
&ClienteBC = new()
&ClienteBC.Nombre = “John Doe”
&ClienteBC.Save()
If &ClienteBC.Success()
	commit
else
	//  Se genera log de los mensajes retornados por el BC
	MessagesLog( &Pgmname, LogLevels.WARN, &ClienteBC.GetMessages())
	
    // Se retorna una mensaje particular al cliente
    MessageAdd(&Messages, “Error al crear el cliente”)
endif
```

**[Volver al inicio](#tabla-de-contenidos)**

## MessagesShow
Lenguajes: Todos
Interfaces: Todos

Propósito
Despliega en pantalla una colección de mensajes.

Tanto si se está utilizando un BC en la interface o si recibimos una colección de mensajes, generalmente tenemos que realizar una recorrida por cada uno para ir mostrando cada mensaje. Esto nos lleva a repetir el mismo código en varios lados e incluir variables extra en nuestras interfaces.

Este procedimiento simplifica ésta tarea.

Sintaxis
SGP_Tools_MessagesShow( Messages-in:&Messages)

&Messages        : Colección de mensajes

Ejemplo
```javascript
&ClienteBC = new()
&ClienteBC.Nombre = “John Doe”
&ClienteBC.Save()

if &ClienteBC.Success()
   SGP_Tools_Commit()
else
   SGP_Tools_MessagesShow.Call(&ClienteBC.GetMessages())
endif
```

**[Volver al inicio](#tabla-de-contenidos)**

## OnlyNumbers
Lenguajes: Todos
Interfaces: Todos

Propósito
Permite extraer los números existentes en un texto.

Sintaxis
SGP_Tools_OnlyNumbers(Character-in:&strin, Character-out:&Numbers)

&strin                   : Texto a procesar
&Numbers         : Números encontrados en &strin

Ejemplo
```javascript
&Numbers  = SGP_Tools_OnlyNumbers(“MI CI ES 1.223.444/6”)

Resultado:
&Numbers  = “12234446”
```

**[Volver al inicio](#tabla-de-contenidos)**

## Repeat
Lenguajes: Todos
Interfaces: Todos

Propósito
Repite una secuencia de caracteres N veces.

Es una extensión de la función "Space" que repetía N veces un espacio.

Sintaxis
SGP_Tools_Repeat( Character-in:&Sequence, Numeric-in:&RepeatTimes, Character-out:&TextOut)

&Sequence: Secuencia de caracteres a repetir
&RepeatTimes: Número de veces para repetir la secuencia

Retorno:

&TextOut           : Cadena de caracteres con la secuencia repetida.

Ejemplo
```javascript
&TextOut = SGP_Tools_Repeat ( “abc.”, 3)

Resultado:
&TextOut = “abc.abc.abc.”
```

**[Volver al inicio](#tabla-de-contenidos)**

## TextTruncate
Lenguajes: Todos
Interfaces: Todos

Propósito
Extra los un conjunto de caracteres de un texto determinado. Se le agrega “…” al final.

Sintaxis
SGP_Tools_TextTruncate( Character-in:&TextIn, Numeric-in:&Len, Character-out:&TextOut)

&TextIn                               : Texto a truncar
&Len                     : Largo con el que deberá quedar el texto

Retorno:

&TextOut           : Texto truncado

Ejemplo
```javascript
&TextOut = SGP_Tools_TextTruncate ( “Truncando un texto libre”, 10)

Resultado:
&TextOut = “Truncan...”
```

**[Volver al inicio](#tabla-de-contenidos)**

## TimeToISO8601
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un DateTime en texto con formato ISO8601.

Más información:
https://en.wikipedia.org/?title=ISO_8601

Sintaxis
SGP_Tools_TimeToISO8601(DateTime-in:&dt, Character-out:&iso)

&dt                        : Fecha y hora a convertir.

Retorno:

&iso                      : Fecha y hora en formato ISO8601

Ejemplo
```javascript
// &DT contiene la fecha hora: 18/06/2015 10:46:13
&IsoText  = SGP_Tools_TimeToISO8601(&dt)

Resultado:
&IsoText contendrá “2015-06-18T10:46:13”.
```

**[Volver al inicio](#tabla-de-contenidos)**

## ValidChars
Lenguajes: Todos
Interfaces: Todos

Propósito
Extrae los un conjunto de caracteres de un texto determinado.

Sintaxis
SGP_Tools_ValidChars( Character-in:&group, Character-in:&textin, Character-out:&validchars)

&group                : Conjunto de caracteres a extraer
&textin                : Texto a revisar

Retorno:

&validchars        : Caracteres extraídos de &textin

Ejemplo
```javascript
&validchars = SGP_Tools_ValidChars(“aeiou”, “esto es una prueba”)

Resultado:
&validchars = “eoeuauea”
```

**[Volver al inicio](#tabla-de-contenidos)**
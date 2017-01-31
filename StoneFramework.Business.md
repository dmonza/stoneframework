# StoneFramework.Business
Funcionalidades relacionadas con el negocio

## Tabla de Contenidos

1. [AmountToText](#amounttotext)
1. [Levenshtein](#levenshtein)
1. Identification
 1. [Identification.ARCBU](#identification.arcbu)
Validación de CBU Argentino
 1. [Identification.ARCUIT_CD](#identification.arcuit_cd)
Dígito de control de CUIT
 1. [Identification.BRCPF_CD](#identification.brcpf_cd)
Dígito de control de CPF
 1. [Identification.UYCI_CD](#identification.uyci_cd)
Dígito de control de CI Uruguay
1. [LuhnCD](#luhncd)
1. [SerialNext](#serialnext)
1. [ValidateEmail](#validateemail)
1. [ValidateID](#validateid)
1. [ValidateImage](#validateimage)
1. [ValidateName](#validatename)

## AmountToText
Lenguajes: Todos
Interfaces: Todos

Propósito
Convierte un importe numérico a letras.

Sintaxis
SGP_Business_AmountToText (Languages-in:&Lenguaje, Numeric-in:&Importe, Character-in:&Moneda, Character-out:&Texto)
&Lenguaje    : Lenguaje del texto generado. Se utiliza el dominio enumerado “Languages”.
&Importe    : Importe a ser convertido a letras.
&Moneda    : Moneda que aparecerá en el importe generado.

Retorno:
&Texto        : Importe en letras

Ejemplo

```javascript
AmountToText ( Languages.Spanish, 1432.32, “USD”, out:&Texto)
&Texto: “UN MIL CUATROCIENTOS TREINTA Y DOS USD CON 32/100 CTVOS.”

AmountToText ( Languages.English, 1432.32, “USD”, out:&Texto)
&Texto: ONE THOUSAND FOUR HUNDRED THIRTY-TWO AND 32/100 USD.

AmountToText (Languages.Portuguese, 1432.32, “USD”, out:&Texto)
&Texto: HUM MIL QUATROCENTOS E TRINTA E DOIS USD E 32/100 CTVOS.
```

**[Volver al inicio](#tabla-de-contenidos)**

## Levenshtein
Lenguajes: Todos
Interfaces: Todas

Propósito
Obtener la [distancia de Levenshtein](https://es.wikipedia.org/wiki/Distancia_de_Levenshtein) entre dos palabras.

La distancia de Levenshtein mide la cantidad de operaciones (sustitución, inserción o eliminación) para convertir una palabra en la otra.

Por ejemplo, la distancia de Levenshtein entre "casa" y "calle" es de 3 porque se necesitan al menos tres ediciones elementales para cambiar uno en el otro.

casa → cala (sustitución de 's' por 'l')
cala → calla (inserción de 'l' entre 'l' y 'a')
calla → calle (sustitución de 'a' por 'e')
Sintaxis
Levenshtein( Character-in:&From, Character-in:&To, Numeric-out:&Distance)

&From        : Palabra origen a comparar

&To          : Palabra a ser comparada

Retorno:

&Distance: Cantidad de operaciones para convertir la palabra &From en &To

Ejemplo
&Distance = Levenshtein( "casa", "cala")

Resultado: &Distance = 1

**[Volver al inicio](#tabla-de-contenidos)**

## Idenfication

### Identification_ARCBU
Lenguajes: Todos
Interfaces: Todos

Propósito
Procedimiento para validar CBU de Argentina.

Es preferente utilizar el procedimiento de validación de documentos [ValidateID](#validateid).

Por más información revisar el siguiente vínculo:
http://es.wikipedia.org/wiki/Clave_Bancaria_Uniforme

**[Volver al inicio](#tabla-de-contenidos)**

### Identification.ARCUIT_CD
Lenguajes: Todos
Interfaces: Todos

Propósito
Calcular el dígito de control para un CUIT Argentino.

Más información en:
http://es.wikipedia.org/wiki/Clave_%C3%9Anica_de_Identificaci%C3%B3n_Tributaria

Sintaxis
SGP_Business_Identificacion_ARCUIT_CD ( Character-in:&CUIT, Numeric-out:&CheckDigit)

&CUIT   : CUIT sobre el que se desea calcular su dígito verificador.

Retorno:

&CheckDigit       : Dígito de control calculado.

Ejemplo
```javascript
ARCUIT_CD (“27-07155257-3”, &CheckDigit)

// &CheckDigit: 3
```

Nota: Para el cálculo del CUIT solo se toman en cuenta los primeros 10 dígitos ya que el último dígito es el dígito de control. En caso de no ser un documento válido se retornará -1.

**[Volver al inicio](#tabla-de-contenidos)**

### Identification.BRCPF_CD
Lenguajes: Todos
Interfaces: Todos

Propósito
Calcular el dígito de control para un CBU de Brasil.

Más información en:

https://en.wikipedia.org/wiki/Cadastro_de_Pessoas_F%C3%ADsicas

Sintaxis
SGP_Business_Identificacion_BRCPF_CD( Character-in:&CPF, Numeric-out:&CheckDigit)

&CPF     : CPF sobre el que se desea calcular su dígito verificador.

Retorno:

&CheckDigit       : Dígito de control calculado.

Ejemplo
```javascript
BRCPF_CD (“27-07155257-3”, &CheckDigit)

// &CheckDigit: 3
```

Nota: Para el cálculo del CPF solo se toman en cuenta los primeros 9 dígitos ya que el resto es equivalen al dígito de control. En caso de no ser un documento válido se retornará -1.

**[Volver al inicio](#tabla-de-contenidos)**

### Identification.UYCI_CD
Lenguajes: Todos
Interfaces: Todos

Propósito
Calcular el dígito de control para una cédula de identidad Uruguaya.

Más información en:

http://es.wikipedia.org/wiki/Documento_de_identidad

Sintaxis
SGP_Business_Identificacion_UYCI_CD ( Character-in:&CI, Numeric-out:&CheckDigit)

&CI        : CI para la cual se desea calcular su dígito verificador.

Retorno:

&CheckDigit       : Dígito de control calculado.

Ejemplo

```javascript
UYCI_CD( “27-07155257-3”, &CheckDigit)

// &CheckDigit: 3
```

Nota: Para el cálculo del CI Uruguayo, se tomarán IDs de 7 u 8 caracteres ya que en el mismo se utilizan los primeros 7. En caso de no ser un documento válido se retornará -1.

**[Volver al inicio](#tabla-de-contenidos)**

## LuhnCD
Lenguajes: Todos
Interfaces: Todos

Propósito
Cálculo de dígito de control basado en el algoritmo de Luhn.

Es útil por ejemplo, para cuando necesitamos verificar el ingreso por teclado de un código por parte de un usuario, de forma de disminuir el error humano.

Para ello, si el código es 12345 y su dígito de control es 5. Cuando se le presenta el código al usuario, se le agrega el dígito de control, ej.: 123455. Si el usuario se equivoca, el código de control variará y por lo tanto se le podrá informar del error.

Para más información, revisar las referencias:
http://en.wikipedia.org/wiki/Luhn_algorithm
https://wiki.openmrs.org/display/docs/Check+Digit+Algorithm

Sintaxis
LuhnCD ( Character-in:&StringIn, Numeric-out:&CheckDigit)

&StringIn            : Texto sobre el cual se calculará el dígito de control.

Retorno:

&CheckDigit       : Dígito de control calculado en base al texto ingresado.

Ejemplo
```javascript
LuhnCD ( “12345”, &CheckDigit)

// &CheckDigit: 5
```

**[Volver al inicio](#tabla-de-contenidos)**

## SerialNext
Lenguajes: Todos
Interfaces: Todos
Versión: 1.01.00

Propósito
Poder generar series alfanuméricas.

Utiles para cuando se necesitan claves para grán volumen de datos.

Sintaxis
SerialNext(Character-in:&SerialTextIn, Character-in:&SerialChars, Numeric-in:&SerialLengthP, Character-out:&SerialTextOut)

&SerialTextIn: Serie de partida

&SerialChars: Conjunto de caracteres para calcular la serie. Si fuera hexadecimal, serían: "01234567890abcdef"

&SerialLengthP: Largo de la serie.

Retorno:

&SerialTextOut: Siguente serie obtenida según la serie original (&SerialTextIn)

Ejemplo

```javascript
&SerialTextIn = "00000000"

for &i=1 to 18
   SerialNext( &SerialTextIn, "01234567890abcdef", 8, &SerialTextOut)
   msg(&SerialTextOut)
endfor

Resultados:
&SerialTextOut = “00000001”
&SerialTextOut = “00000002”
&SerialTextOut = “00000003”
&SerialTextOut = “00000004”
&SerialTextOut = “00000005”
&SerialTextOut = “00000006”
&SerialTextOut = “00000007”
&SerialTextOut = “00000008”
&SerialTextOut = “00000009”
&SerialTextOut = “0000000a”
&SerialTextOut = “0000000b”
&SerialTextOut = “0000000c”
&SerialTextOut = “0000000d”
&SerialTextOut = “0000000e”
&SerialTextOut = “0000000f”
&SerialTextOut = “00000010”
&SerialTextOut = “00000011”
&SerialTextOut = “00000012”
```

**[Volver al inicio](#tabla-de-contenidos)**

## ValidateEmail
Lenguajes: Todos
Interfaces: Todos

Propósito
Procedimiento para la validación de una dirección de E-mail.

Está basado en la validación mediante expresiones regulares definida en:

http://wiki.genexus.com/commwiki/servlet/hwiki?Regular+Expressions+%28RegEx%29

Sintaxis
ValidateEmail(Character-in:&Email, Boolean-out:&IsValid)

&Email                 : Dirección de e-mail a validar.

Retorno:

&IsValid               : Boolean especificando si la dirección es válida o no.

Ejemplo
```javascript
ValidateEmail (“johndoe@dummy.com”, &IsValid)

&IsValid = true
```

**[Volver al inicio](#tabla-de-contenidos)**

## ValidateID
Lenguajes: Todos
Interfaces: Todos

Propósito
Valida un tipo de documento de identidad definido en el dominio enumerado “IdentificationTypes”.

Uruguayan CICPF BrasilCUIT ArgentinaCBU Argentina

Sintaxis
SGP_Business_ValidateID (IdentificationTypes-in:&IDType, Character-in:&ID_Number, Boolean-out:&IsValid)

&IDType              : Tipo de documento a validar. Basado en “IdentificationTypes”.
&ID_Number    : Número de documento a validar. Character(20).

Retorno:

&IsValid               : Boolean que indica si el documento es válido o no.

Ejemplo
```javascript
SGP_Business_ValidateID(IdentificationTypes.UruguayanCI, “55996612”, &IsValid)

&IsValid: true

SGP_Business_ValidateID (IdentificationTypes.UruguayanCI, “55996616”, &IsValid)

&IsValid: false
```

**[Volver al inicio](#tabla-de-contenidos)**

## ValidateImage
Lenguajes: Todos
Interfaces: Todos

Propósito
Procedimiento que chequea si un archivo es una imagen.

Se chequean las extensiones: jpg, png, gif, tif

Sintaxis
ValidateImage(Character-in:&FileName, Boolean-out:&IsValid)

&FileName: Nombre del archivo

Retorno:

&IsValid               : Boolean especificando si es una imagen o no.

Ejemplo
```javascript
SGP_Business_ValidateImage(“myimagen.jpg”, &IsValid)

Resultado:
&IsValid = true

SGP_Business_ValidateImage(“myreporte.pdf”, &IsValid)

Resultado:
&IsValid = false
```

**[Volver al inicio](#tabla-de-contenidos)**

## ValidateName
Lenguajes: Todos
Interfaces: Todos

Propósito
Procedimiento para la validación de una dirección un nombre.

La validación chequea que existan al menos dos palabras separadas por un espacio.

Sintaxis
SGP_Business_ValidateName ( Character-in:&Name, Boolean-out:&IsValid)

&Email                 : Nombre a validar.

Retorno:

&IsValid               : Boolean especificando si el nombre es válido o no.

Ejemplo
```javascript
SGP_Business_ValidateName ( “John Doe”, &IsValid)

&IsValid = true
```

**[Volver al inicio](#tabla-de-contenidos)**


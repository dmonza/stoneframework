# StoneFrameworkPatterns.SerialPattern
El patrón "Serials" nos permite administrar la clasica tabla de "numeradores" con la ventaja de poder manejar series alfanumericas y dentro de ellas, la particularidad de poder utilizar series exadecimales también.

## Estructuras
Utiliza una única tabla (SGP_Serials) en la cual se defini un identificador de la serie (SerialEntity), ej. "CLIENTE" y luego, el último valor de la serie se almacena en SeiralNumeric (si es numérica ) y en SerialText (si es alfanumérica).

## Funcionamiento
Para el funcionamiento, tenemos tres procedimientos dependiendo del tipo de serie que queremos generar.

### SerialNumericNext

Obtiene el siguiente número de una serie numérica.

Sintaxis

SerialNumericNext(Character-in:&SerialEntity, Numericout:&NextOut)

&SerialEntity: Clave de serialización

Retorno:

&NextOut: Siguente valor de la serie según el último valor que estaba almacenado.

Ejemplo

&NuevoID = SerialNumericNext("CLIENTE-ID")

Resultado:

&NuevoID contendrá el un nuevo ID numérico de cliente.

### SerialTextNext

Obtiene el siguiente valor de una serie alfanumérica.

Por detalles del funcionamiento, revisar el procedimiento SGP_Business_Serial

Sintaxis

SerialTextNext(Character-in:&SerialEntity, Character-in:&SerialChars, Character-in: &Prefix, Numeric-in:&SerialLengthP, Character-out:&SerialTextOut)

&SerialEntity: Clave de serialización

&Prefix: Premite agregar un prefijo a la serie. Se genera según la mascara [prefijo]-[valor]

&SerialChars: Conjunto de caracteres para calcular la serie. Si fuera hexadecimal, serían: "01234567890abcdef"

&SerialLengthP: Largo de la serie a generar.

Retorno:

&SerialTextOut: Siguente valor de la serie según el último valor que estaba almacenado.

Ejemplo

Si el último valor almacenado de la serie "abc" es "aab"

&Valor = SerialTextNext( "PROD-SERIE", "abc","", 3)

Resultado:

&Valor = "aac"

### SerialHexNext

Ésta función permite generar una serie hexadecimal. Utiliza el procedimiento SerialTextNext estableciendo el conjunto de caracteres "01234567890abcdef".

Se puede configurar el largo de la serie en el procedimiento "StoneFrameworkConfig".

Sintaxis

SerialTextNext(Character-in:&SerialEntity, Character-in: &Prefix, Character-out:&SerialTextOut)

&SerialEntity: Clave de serialización

&Prefix: Premite agregar un prefijo a la serie. Se genera según la mascara [prefijo]-[valor]

Retorno:

&SerialTextOut: Siguente valor de la serie hexadecimal según el último valor que estaba almacenado.
# StoneFrameworkPatterns.Files
El patrón de archivos pretende resolver la gestión de archivos en los sistemas donde es necesario interactuar con estos. Existe gran cantidad de sistemas donde los archivos son almacenados en diferentes tablas de la DB según la entidad.

Por ejemplo, muchas veces, la foto del cliente se almacenaba en la tabla de clientes, la del artículo en la tabla de artículos, etc. Esto genera que se deben generar código particular para gestionar los archivos en cada caso.

Sumado a lo anterior, hay varias formas de almacenar los archivos: Base de Datos (blob), filesistem (disk), amazon s3, drop box, etc.

Según lo anterior, una vez que nos cazamos con una forma de almacenamiento era complicado cambiar por todos los puntos donde se debían hacer cambios.

Para más información sobre el funcionamiento, visitar: Patrón de archivos

## Configuración
La configuración se realizará mediante StoneFrameworkConfig.

Por defecto se establecen las rutas:

PublicFiles: Está dentro de la carpeta web ya que es donde se almacenan los archivos públicos.
..\PrivateFiles: Como se puede ver, está antes de nuestra carpeta web y es donde se almacenan los archivos privados.

Se deben tener configuradas correctamente en Genexus las carpetas:

Temp Media Directory Property
Blob Local Storage Directory

Estas carpetas deben tener acceso de lectura/escitura por el usuario asignado a nuestra aplicación.

Nora: Por razones de seguridad, conviene que la carpeta PrivateTempStorage no sea pública (carpeta web), por lo que podrá estar en "..\PrivateTempStorage".

## Funcionamiento
Para las estructuras que se necesiten archivos, se crearán claves foráneas a FileID mediante subtipo.

Por ejemplo, si queremos agregar a nuestros clientes una foto, en la transacción de clientes crearemos CliFileID subtipo de FileID. Luego en el momento de cargar la imagen se llamará al proc. FileAdd que se explica a continuación.

### Agregar un nuevo archivo

FileAdd(Character-in:&FileName, Character-in:&FileFullName, Boolean-in:&IsPublic, FileLocations-in:&FileLocation, CommonResponse-out:&CommonResponse)

&FileName: Nombre real del archivo
&FileFullName: Ruta de acceso al archivo
&IsPublic: El archivo debe ser público, accedido por cualquiera?
&FileLocation: Donde estará almacenado el archivo?

Retorno

&CommonResponse: Retornará el ID del archivo creado en &CommonResponse.ID o los mensajes si ocurrió algún problema.

### Eliminar un archivo

FileDelete(Numeric-in:FileID, Boolean-in:&Force)

FileID: ID del archivo a eliminar.
&Force: Eliminación lógica o física.

### Obtener la ruta física a un archivo

FileFullNameGet(Numeric-in:FileID, Character-out:&FullName)
FileID: Archivo para el cual se quiere obtener la ruta completa.

Retorno:

&FullName: Ruta completa al archivo especificado.

### Obtener una URL de acceso al archivo

Con FileUrlGet obtendremos la Url al archivo.

En el caso de ser un archivo público, la Url será válida mientras exista el archivo. Si se trata de un archivo privado, la Url será válida durante la cantidad de segundos definida en el dominio enumerado StoneFrameworkConfig.

FileUrlGet(Numeric-in:FileID, Boolean-in:&IsDownload, Character-out:&URL)

FileID: Archivo del cual se desea obtener una Url.
&IsDownload: El archivo se debe descargarse (true) o mostrarse en línea(false)?

Retorno:

&URL: Url de acceso al archivo especificado.
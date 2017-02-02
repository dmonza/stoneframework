# StoneFramework.Net

## Tabla de Contenidos

1. [BrowserLanguages](#browserlanguages)
1. [EmailSend](#emailsend)
1. [FileAccessGet](#fileaccessget)
1. [FileGet](#fileget)
1. [RemoteAddr](#remoteaddr)
1. [SitemapGenerate](#sitemapgenerate)
1. [SitemapUrlGet](#sitemapurlget)
1. [UrlToLocation](#urltolocation)

## BrowserLanguages
Lenguajes: Todos
Interfaces: Web

**Propósito**
En los sistemas web, es útil conocer el lenguaje utilizado por nuestros usuarios a fin de seleccionar el lenguaje por defecto para nuestra aplicación.

Mediante esta función podemos obtener los lenguajes de preferencia configurados en el browser de nuestros usuarios.

Por otro lado, la función tiene la particularidad de mapear el lenguaje del browser al lenguaje Genexus. Ej.:

Si el browser tiene configurado "es", éste está vinculado a "Spanish".

**Sintaxis**

BrowserLanguages( LanguageType[collection]-out:&LanguageList)

Retorno:

&LanguageList: Lista de lenguajes configurados en el browser del usuario.

**Ejemplo**

```javascript
&LanguageList = BrowserLanguages()

Resultado:

es, gx: Spanish
en, gx: English
```

**[Volver al inicio](#tabla-de-contenidos)**

## EmailSend
Lenguajes: Todos
Interfaces: Todos

**Propósito**
Envío de un mail utilizando los tipos de datos &SmtpSession y &MailMessage.

Como se verá en la sintaxis, este procedimiento carga la configuración del servidor de correo desde StoneFrameworkConfig para realizar el envío de mail.

Referencias:
http://wiki.genexus.com/commwiki/servlet/wiki?SMTPSession+Data+Type
http://wiki.genexus.com/commwiki/servlet/wiki?MailMessage+Data+Type

**Sintaxis**
EmailSend( MailMessage-in:&Mail, Messages.Message-out:&Message)

&Mail                                   : Mensaje de email basado en MailMessage.

Retorno:

&Message                         : Retorna si ocurrió o no un error al enviar el e-mail.

&Message.Id              : Id retornado por SmtpSession
&Message.Description : Descripción del error

**Ejemplo**

```javascript
&MailMessage.From.Name = "My Name"
&MailMessage.From.Address = "myaccount@gmail.com"
&MailMessage.To.New( "John Doe", "johndoe@gmail.com")
&MailMessage.Subject  = "EmailSend testing"
&MailMessage.HTMLText = "EmailSend works great!"

EmailSend.Call( &MailMessage, &Message)

if &Message.Description.IsEmpty()
   msg("Email sended!")
else
   msg( &Message.Description)
endif
```

**[Volver al inicio](#tabla-de-contenidos)**

## FileAccessGet
Lenguajes: Todos
Interfaces: Web

**Propósito**

Crea una sesión identificada por una clave la cual contiene la información de un archivo para ser descargado.

Funciona en conjunto con el procedimiento FileGet el cual recibe dicha clave y despliega en línea o descarga un archivo.

Utilizando estos procedimientos se evita por ejemplo, definir reportes como main y callprotocol http.

**Sintaxis**

FileAccessGet( Character-in:&FullName, Character-in:&Name, Boolean-in:&Download, Numeric-in:&TimeoutSeconds, Character-out:&Token)

&FullName         : Ruta completa al archivo.

&Name                               : Friendly name o nombre con el cual se le descargará el archivo al usuario.

&Download       : Indica si el archivo se descargará o se mostrará en línea.

&TimeoutSeconds          : Tiempo en segundos durante el cual podrá ser utilizado la clave generada.

Retorno:

&Token               : Clave identificando websession donde se almacenan los parámetros anteriores.

**Ejemplo**

```javascript
FileAccessGet.Call ( “c:\[mytempfiles]\temp_myreport.pdf”, "myreport.pdf", true, 30, &token)

&token = “848e3d24-e945-47ad-ab86-7d95e8fc5945”
```

**[Volver al inicio](#tabla-de-contenidos)**

## FileGet
Lenguajes: Todos
Interfaces: Web

**Propósito**

Descarga o visualiza un archivo en línea.

Funciona en conjunto con el procedimiento [FileAccessGet](#fileaccessget) quién se encarga de generar la clave de acceso al archivo.

Utilizando estos procedimientos se evita por ejemplo, definir reportes como main y callprotocol http.

Se obtendrá la Url de descarga mediante el método “link” de FileGet.

**Sintaxis**

FileGet( Character-in:&Token)

&Token                               : Clave de acceso a los datos del archivo.

**Ejemplo**

```javascript
// El token “848e3d24-e945-47ad-ab86-7d95e8fc5945” fue previamente obtenido mediante
// el proc. FileAccessGet

// La invoación se puede hacer de dos formas
// 1. Mediante Call
FileGet.Call( “848e3d24-e945-47ad-ab86-7d95e8fc5945”)

// 2. Creando un link y utilizandolo en diferentes lugares
&Link = FileGet.Link( “848e3d24-e945-47ad-ab86-7d95e8fc5945”)

EmbeddedPage.Source = &Link
MyTextBlock.Link = &Link
```

Nota: Si se realiza la invocación con call, en algunos navegadores puede redirigir a una página en blanco o puede quedar la pantalla de carga en el webpanel.

**[Volver al inicio](#tabla-de-contenidos)**

## RemoteAddr
Lenguajes: Todos
Interfaces: Web

**Propósito**

Al igual que la función RemoteAddr(), este procedimiento obtiene la IP del equipo remoto pero con la particularidad de revisar algunos headers particulares para ambientes donde se trabaja con un balanceador de carga Ej. Amazon Elastic Load Balancing.

Actualmente se revisa el header: HTTP_X_FORWARDED_FOR

Se estudiará revisar otros headers como: HTTP_CLIENT_IP;HTTP_X_FORWARDED;HTTP_X_CLUSTER_CLIENT_IP;HTTP_FORWARDED_FOR;HTTP_FORWARDED;REMOTE_ADDR

**Sintaxis**

RemoteAddr( Character-out:&RemoteIP )

Retorno:

&RemoteIP: IP del equipo remoto.

**Ejemplo**

```javascript
&RemoteIP = RemoteAddr()

Resultado:

&RemoteIP = “123.123.123.123”
```

**[Volver al inicio](#tabla-de-contenidos)**

## SitemapGenerate
Lenguajes: Todos
Interfaces: Todos

**Propósito**
Generar un XML Sitemap.

El sitemap sirve para publicar nuestras páginas web en los buscadores a fin de indexarlas. Muchas de las páginas no son indexadas por la dificultad de acceder a ellas y mediante este invento de Google se le simplifica la tarea a los buscadores.

Por más información:
https://en.wikipedia.org/wiki/Site_map
https://support.google.com/webmasters/answer/183668?hl=en

**Sintaxis**
SitemapGenerate ( SitemapUrl[collection]-in:&Urls, Character-out:&SitemapXML)

&Urls                    : Lista de objetos “SitemapUrl “.
SitemapUrl

loc: Url a indexar.
lastmod: Ultima modificación
changefreq: Cuan frecuentemente cambia (siempre, cada hora, diaria, semanal, etc.)
priority: Prioridad de la página para nuestro sitio (0.1 a 1)
Retorno:

&SitemapXML  : Se retorna el sitemap en XML.

**Ejemplo**

```javascript
// &Url del tipo SitemapUrl
// &Urls es una lista de SitemapUrl

&Url = new()
&Url.loc = !http://www.gprojex.com/home.aspx
&Url.changefreq = SitemapFrequencies.always
&Url.priority = 0.5
&Url.lastmod = &Today
&Urls.Add(&Url)

&StringXML = SitemapGenerate(&Urls)

Resultado:

<?xml version = "1.0" encoding = "utf-8"?>
<urlset xmlns="http://www.sitemaps.org/schemas/sitemap/0.9" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:schemaLocation="http://www.sitemaps.org/schemas/sitemap/0.9 http://www.sitemaps.org/schemas/sitemap/0.9/sitemap.xsd">
    <url>
        <loc>http://www.gprojex.com/home.aspx</loc>
        <lastmod>2015-06-16</lastmod>
        <changefreq>always</changefreq>
        <priority>0.5</priority>
    </url>
</urlset>
```

**[Volver al inicio](#tabla-de-contenidos)**

## SitemapUrlGet
Lenguajes: Todos
Interfaces: Todos

**Propósito**

Procedimiento accesorio para obtener rápidamente una variable del tipo SDT:SitemapUrl con los datos requeridos.

Para más información, revisar el procedimiento [SitemapGenerate](#sitemapgenerate).

**Sintaxis**

SitemapUrlGet (Character-in:&loc, Date-in:&lastmod, SitemapFrequencies-in:&changefreq, Numeric-in:&priority, SitemapUrl-out:&Url)

&loc                   : Url a indexar.
&lastmod            : Última modificación
&changefreq       : Cuan frecuentemente cambia (siempre, cada hora, diaria, semanal, etc.)
&priority             : Prioridad de la página para nuestro sitio (0.1 a 1)

Retorno:

&Url      : Se retorna el SitemapUrl representando los parámetros recibidos.

**Ejemplo**

```javascript
// &Url del tipo SitemapUrl
// &Urls es una lista de SitemapUrl

SitemapUrlGet.Call(!"http://www.gprojex.com/home.aspx", &Today, SitemapFrequencies.always,0.5, &Url)

&Url es una variable basada en el SDT Sitemap.
```

Nota: Se entiende que la &Url se agregará a una lista de Urls para luego utilizar el proc. [SitemapGenerate](#sitemapgenerate).

**[Volver al inicio](#tabla-de-contenidos)**

## UrlToLocation
Lenguajes: Todos
Interfaces: Todos

**Propósito**

Permite generar una variable del tipo “Location” desde una Url.

Generalmente cunado trabajamos con Webservices es necesario crear un Location para variar la Url en tiempo de ejecución.

Particularmente el tipo de datos location es engorroso de configurar ya que hay que separar la Url en sus elementos básicos (Host, Port, BaseUrl, ResourceName, etc ).

Para más información:
http://wiki.genexus.com/commwiki/servlet/wiki?Location+Data+Type
http://wiki.genexus.com/commwiki/servlet/wiki?GetLocation+Function
http://wiki.genexus.com/commwiki/servlet/wiki?Location+property

**Sintaxis**

UrlToLocation( Character-in:&Url, Character-in:&LocationName, Location-out:&Location)

&Url: Url a convertir en Location
&LocationName: Nombre del “location” asociado a nuestro webservice.

Retorno:

&Location: Variable del tipo Location representando nuestra &Url.

**Ejemplo**

```javascript
// &Location : Variable del tipo Location

UrlToLocation ( "https://dummy.com/api/myservice.aspx", in:&LocationName, out:&Location)

// Resultado:
// &Location representando la &Url especificada.
```

**[Volver al inicio](#tabla-de-contenidos)**
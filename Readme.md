# ![Sifei](https://www.sifei.com.mx/web/image/res.company/1/logo?unique=38c7250)




# JAR para consumo  de WS SOAP de timbrado de Sifei utilizando axis2

Este repositorio incluye el JAR con los artefactos necesarios para consumir el WS de timbrado, solo debes importarlo en tu proyecto.

URL de pruebas:

http://devcfdi.sifei.com.mx:8080/SIFEI33/SIFEI?wsdl


Puedes utilizar el JAR incluido, o bien, generarlo tu mismo y para luego importarlo en tu proyecto Java.


## Descargar JAR
Puedes descargar directamente el JAR:

[Descargar JAR aquí](https://github.com/SifeiMexico/timbradoJarAxis2/raw/master/target/mx.com.sifei-1.0.0.jar)


## Generación de JAR (opcional)
 
Para generar el JAR tu mismo debes descargar el proyecto y ejecutar los siguientes comandos, es necesario que tengas al menos *maven* instalado:



```shell
#clonar proyecto
git clone git@github.com:SifeiMexico/timbradoJarAxis2.git
#generar Jar
mvn package

```
El comando  de arriba generara los artefactos basado el archivo sifei.wsdl que contiene la definicion de los servicios, este wsdl debe ser el mas reciente.

## Ejemplo de timbrado usando este jar:
Tras importar el jar generado en este proyecto podras invocar todos los servicios expuestos. A continuacion se muestra como invocar getCFDI para timbrar un CFDI, el cual es solo uno de los varios metodos expuestos.

```java
       
try {
SIFEIServiceStub serviceStub = null;

String usuarioSIFEI = "usuarioSIFEI";
String passwordSIFEI = "password SIFEI";
String serie = "";//vacia para usuarios de timbrado
String pathXML = "Path de XML";
String idEquipo = "Id de equipo proporcionado por correo electrónico";

//Esta es la url del ambiente de pruebas
String urlTimbrado = "http://devcfdi.sifei.com.mx:8080/SIFEI33/SIFEI?wsdl";


serviceStub = new SIFEIServiceStub( urlTimbrado );

var document= GetCFDIDocument.Factory.newInstance();
var getCFDI=document.addNewGetCFDI();
//Llenado de parametros obligatorios que tienen que enviarse

byte[] bFile = Files.readAllBytes(Path.of("./assets/xml_sellado.xml"));
getCFDI.setArchivoXMLZip(bFile);
getCFDI.setIdEquipo(idEquipo);
getCFDI.setPassword(passwordSIFEI);
getCFDI.setSerie(serie);
getCFDI.setUsuario(usuarioSIFEI);      
//se invoca al metodo
var getCFDIResponseE = serviceStub.getCFDI(document);
//se extrae la respuesta
var getCFDIResponse = getCFDIResponseE.getGetCFDIResponse();


//La respuesta es un zip, se obtienen los bytes del zip que contiene el XML.
String XMLTimbrado = getFileFromZipBytes(getCFDIResponse.getReturn());
System.out.println("\n"+XMLTimbrado);

} catch (Exception ex) {
//Captura del Resquest
System.out.println("\n:::::EXCEPCION:::::");
ex.printStackTrace();

}finally {
System.out.println("\n:::::SOAP REQUEST:::::");
serviceStub._getServiceClient().getLastOperationContext().getMessageContext("Out").getEnvelope().serialize(System.out);

System.out.println("\n:::::SOAP RESPONSE:::::");
serviceStub._getServiceClient().getLastOperationContext().getMessageContext("In").getEnvelope().serialize(System.out);

}
```
Si deseas ver un ejemplo completo favor de ver:
{enlace del proyecto}

## Dependencias maven
Tras importar el JAR en tu proyecto, dentro de tu archivo pom deberas agregar las siguiente dependencias.


```xml
<!--dependencias para consumir ws-->
<dependency>
    <groupId>javax.activation</groupId>
    <artifactId>activation</artifactId>
    <version>1.1.1</version>
</dependency>
<dependency>
    <groupId>org.apache.ws.commons.axiom</groupId>
    <artifactId>axiom-api</artifactId>
    <version>1.2.20</version>
</dependency>
<dependency>
<groupId>org.apache.ws.commons.axiom</groupId>
<artifactId>axiom-impl</artifactId>
<version>1.2.20</version>
</dependency>
<dependency>
<groupId>org.apache.axis2</groupId>
<artifactId>axis2-adb</artifactId>
<version>1.7.7</version>
</dependency>
<dependency>
<groupId>org.apache.axis2</groupId>
<artifactId>axis2-transport-local</artifactId>
<version>1.7.7</version>
</dependency>
<dependency>
<groupId>org.apache.axis2</groupId>
<artifactId>axis2-xmlbeans</artifactId>
<version>1.7.7</version>
</dependency>
<dependency>
<groupId>org.apache.axis2</groupId>
<artifactId>axis2-kernel</artifactId>
<version>1.7.7</version>
</dependency>
<dependency>
<groupId>org.apache.axis2</groupId>
<artifactId>axis2-transport-http</artifactId>
<version>1.7.7</version>
</dependency>
<dependency>
<groupId>commons-httpclient</groupId>
<artifactId>commons-httpclient</artifactId>
<version>3.1</version>
</dependency>
<dependency>
<groupId>org.apache.neethi</groupId>
<artifactId>neethi</artifactId>
<version>3.0.3</version>
</dependency>
<dependency>
<groupId>org.apache.woden</groupId>
<artifactId>woden-core</artifactId>
<version>1.0M10</version>
</dependency>
<dependency>
<groupId>org.apache.ws.xmlschema</groupId>
<artifactId>xmlschema-core</artifactId>
<version>2.2.1</version>
</dependency>
```



## ¿Necesitas mas ejemplos?
Si necesitas más ejemplos del resto de servicios favor de generar un nuevo issue

## Mantente en contacto

- [Twitter](https://twitter.com/sifeimexico)
- [Youtube](https://www.youtube.com/sifeimexico)
- [Facebook](https://www.facebook.com/sifeimexico)
- [Sitio Oficial](https://www.sifei.com.mx/)


# ![Sifei](https://www.sifei.com.mx/web/image/res.company/1/logo?unique=38c7250)




# Generador de clases para consumo  de WS SOAP de timbrado de Sifei

Este repositorio incluye el jar que incluye las los artefactos necesarios para consumir el WS de timbrado

http://devcfdi.sifei.com.mx:8080/SIFEI33/SIFEI?wsdl


Puedes utilizar el jar incluido o bien generarlo tu mismo y luego importarlo en tu proyecto Java.


## Generación de JAR
 
Para ejecutar el jar tu mismo debes descargar el proyecto y ejecutar:



```shell
git clone {incluir url}
mvn package

```
El comando  de arriba generara los artefactos basado el archivo sifei.wsdl que contiene la definicion de los servicios, este wsdl debe ser el mas reciente.

## Ejemplo de timbrado usando este jar:
Tras importar el jar generado en este proyecto podras invocar todos los servicios expuestos. A continuacion se muestra como invocar getCFDI para timbrar un Cfdi.

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


## ¿Necesitas mas ejemplos?
Si necesitas más ejemplos del resto de servicios favor de generar un nuevo issue

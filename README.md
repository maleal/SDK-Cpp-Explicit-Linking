# SDK-Cpp-Explicit-Linking
SDK-C++ de Vinculación Explícita

<a name="inicio"></a>		
SDK C++ 	
==================	
		
Modulo de conexión con el gateway de pago 'Todo Pago'		

######[Instalación](#instalacion)		
######[Generalidades](#general)	
######[Uso](#uso)		
######[Datos adicionales para prevencion de fraude] (#datosadicionales) 		
######[Ejemplo](#ejemplo)		
######[Modo test](#test)
######[Status de la operación](#status)
######[Tablas de referencia](#tablas)

<a name="Instalación"></a>		
## Instalación
<ul>
<li>a. Se debe descargar la última versión del SDK desde el botón Download ZIP, branch master.		
   Una vez descargado y descomprimido, debe incluir el archivo TPConnectorDll.h a su proyecto.
</li>
<li>b. Deberá definir la macro "DLL_EXPLICIT_LINK" para todas las configuraciones de su proyecto. Si usa Visual C++ 2012            deberá     ir a: Property Pages -> C/C++ -> Preprocessor y seleccionar "All Configurations" para luego en          "Preprocessor Definitios"     crear la macro.
</li>
<li>c. Archivo 'cacerts.pem' Contiene los certificados públicos otorgados por una autoridad de certificación de confianza.         puede descargarlo desde [aquí](http://curl.haxx.se/docs/caextract.html/).
</li>
</ul>

<br />		
[<sub>Volver a inicio</sub>](#inicio)

<a name="general"></a>
## Generalidades
Esta versión soporta únicamente pago en moneda nacional argentina (CURRENCYCODE = 32).
[<sub>Volver a inicio</sub>](#inicio)

<a name="uso"></a>		
## Uso		
####1.Inicializar la clase correspondiente al conector (TodoPago).

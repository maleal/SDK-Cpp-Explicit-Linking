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
####1.Cargar la Dll y obtener el puntero a una instancia de la interfaz (TodoPago).
	Para esto deberá usar la función API LoadLibrary() y a GetProcAddress() ej:
	<br />
	##	HMODULE hDll = LoadLibrary( "ExLinkTPConnectorDll.dll" );
	##	if (hDll)
	##		PF_GetDLLInterface pIntf = (PF_GetDLLInterface) GetProcAddress(hDll, "GetDLLInterface");
####2.Inicializar el conector (TodoPago).
	Al puntero a funcion obtenido, asignelo a uno del tipo de la clase 'TPCtor_Interface' con el cual accederá a todas
	las operaciones de la interfaz pero primero deberá inicializar el conector con el metodo 'TPConnector_Init' de la
	siguiente forma:
	 -crear un string con el Endpoint suministrados por Todo Pago
	 -crear un string Athorization con el dato del http header suministrados por Todo Pago
	 -llame al metodo 'TPConnector_Init'
	 <br />
	##	TPCtor_Interface *pConntor = pIntf();
	## 	if( pConntor ) {
			cout << "----------------Calling SendAuthorizeRequest() ........." << endl;
			if(ret = pConntor->TPConnector_Init(Endpoint, Athorization) == 0)
				cout << "---------------- SendAuthorizeRequest() OK\n";


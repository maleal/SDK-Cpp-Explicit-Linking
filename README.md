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
####3.Servicio Web 'Authorize Request' (TodoPago).
	Antes de consumir el servicio web 'AuthorizeRequest' debemos invocar al método
	'SendAuthorizeRequest_SetParams()' este metodo usa dos parámetros del tipo std::map, para cada uno de estos parámetros estan definidas las keys y se usan de la siguiente forma:
	<ul>
		<li> 'RParams' es el primer parametro del metodo </li>
		     ejemplo:
		     map<std::string, std::string>RParams;
		     RParams[SECURITY]	= "912EC803B2CE49E4A541068D495AB570";
		     RParams[SESSION]	= "ABCDEF-1234-12221-FDE1-00000200";
	             RParams[MERCHANT]	= "2153";
	             RParams[URL_OK]	= "http://google.com";
	             RParams[URL_ERROR]	= "http://www.coco.com";
	             RParams[ENCODING_METHOD] = "XML";
	        
	        <li> 'RPayload' es el segundo parametro de este metodo y también del tipo std:map<string, string> </li>
	             ejemplo:
	                map<string, string>PayLParams;
	                PayLParams[EMAILCLIENTE]	= "client_email@dominio.com";
			PayLParams[AMOUNT]		= "55";
			PayLParams[CURRENCYCODE]	= "032";
			PayLParams[OPERATIONID]		= "01";
			PayLParams[PAYL_MERCHANT]	= "2153";
		
			PayLParams[CSBTCITY]		= "Villa General Belgrano"; //MANDATORIO.
			PayLParams[CSSTCITY	]	= "Villa General Belgrano"; //MANDATORIO.
		           
			PayLParams[CSBTCOUNTRY]		= "AR";//MANDATORIO. Código ISO.
			PayLParams[CSSTCOUNTRY]		= "AR";//MANDATORIO. Código ISO.
		             
			PayLParams[CSBTEMAIL]		= "todopago@hotmail.com"; //MANDATORIO.
			PayLParams[CSSTEMAIL]		= "todopago@hotmail.com"; //MANDATORIO.
		             
			PayLParams[CSBTFIRSTNAME]	= "Juan";//MANDATORIO.      
			PayLParams[CSSTFIRSTNAME]	= "Juan";//MANDATORIO.      
		             
			PayLParams[CSBTLASTNAME]	= "Perez";//MANDATORIO.
			PayLParams[CSSTLASTNAME]	= "Perez";//MANDATORIO.
		             
			PayLParams[CSBTPHONENUMBER]	= "541160913988";//MANDATORIO.     
			PayLParams[CSSTPHONENUMBER]	= "541160913988";//MANDATORIO.     
		             
			PayLParams[CSBTPOSTALCODE]	= " 1010";//MANDATORIO.
			PayLParams[CSSTPOSTALCODE]	= " 1010";//MANDATORIO.
		             
			PayLParams[CSBTSTATE]		= "B";//MANDATORIO
			PayLParams[CSSTSTATE]		= "B";//MANDATORIO
		             
			PayLParams[CSBTSTREET1]		= "Cerrito 740";//MANDATORIO.
			PayLParams[CSSTSTREET1]		= "Cerrito 740";//MANDATORIO.
		             
			PayLParams[CSBTCUSTOMERID]	= "453458";; //MANDATORIO.
			PayLParams[CSBTIPADDRESS]	= "192.0.0.4"; //MANDATORIO.       
			PayLParams[CSPTCURRENCY]	= "ARS";//MANDATORIO.      
			PayLParams[CSPTGRANDTOTALAMOUNT]= "125.38";//MANDATORIO.
			PayLParams[CSMDD7		]	="";//NO MANDATORIO.        
			PayLParams[CSMDD8		]	="Y"; //NO MANDATORIO.       
			PayLParams[CSMDD9		]	="";//NO MANDATORIO.       
			PayLParams[CSMDD10		]	="";//NO MANDATORIO.      
			PayLParams[CSMDD11		]	="";//NO MANDATORIO.
			PayLParams[STCITY		]	="rosario";//MANDATORIO.       
		
			PayLParams[STCOUNTRY	]		="";//MANDATORIO.      
			PayLParams[STEMAIL		]	="jose@gmail.com";//MANDATORIO.        
			PayLParams[STFIRSTNAME	]		="Jose";//MANDATORIO.        
			PayLParams[STLASTNAME	]		="Perez";//MANDATORIO.      
			PayLParams[STPHONENUMBER]		= "541155893737";//MANDATORIO.        
			PayLParams[STPOSTALCODE	]		= "1414";//MANDATORIO.        
			PayLParams[STSTATE		]	="D";//MANDATORIO     
		
			PayLParams[STSTREET1	]		="San Martín 123";//MANDATORIO.       
			PayLParams[CSMDD12		]	= "";//NO MADATORIO.     
			PayLParams[CSMDD13		]	= "";//NO MANDATORIO.     
			PayLParams[CSMDD14		]	= "";//NO MANDATORIO.      
			PayLParams[CSMDD15		]	= "";//NO MANDATORIO.        
			PayLParams[CSMDD16		]	= "";//NO MANDATORIO.
		
			PayLParams[CSITPRODUCTCODE] 		= "electronic_good";//CONDICIONAL
			PayLParams[CSITPRODUCTDESCRIPTION]	= "NOTEBOOK L845 SP4304LA DF TOSHIBA";//CONDICIONAL.     
			PayLParams[CSITPRODUCTNAME] 		= "NOTEBOOK L845 SP4304LA DF TOSHIBA";//CONDICIONAL.  
			PayLParams[CSITPRODUCTSKU]		= "LEVJNSL36GN";//CONDICIONAL.      
			PayLParams[CSITTOTALAMOUNT] 		= "1254.40";//CONDICIONAL.      
			PayLParams[CSITQUANTITY]		= "1";//CONDICIONAL.       
			PayLParams[CSITUNITPRICE]		= "1254.40";
	</ul>
	
	


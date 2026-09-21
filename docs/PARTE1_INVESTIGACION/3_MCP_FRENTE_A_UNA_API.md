3. MCP frente a una API
	3.1 ¿Qué es una API?

		Una API (Application Programming Interface) es un mecanismo mediante el cual diferentes programas pueden comunicarse entre sí siguiendo un conjunto de reglas previamente 		definido.

		En una API web, por ejemplo, el desarrollador consulta la documentación del servicio para conocer qué operaciones están disponibles, qué endpoints existen, qué parámetros 		debe enviar y qué formato tendrá la respuesta.

		Después, el desarrollador escribe código que realiza esas llamadas. Por ejemplo, una aplicación podría tener programado que cuando el usuario presione un botón se realice 		una petición determinada:

			Usuario ----> Presiona botón ----> Código de la aplicación ----GET /usuarios/123----> API ----> Respuesta ----> Código interpreta la respuesta

		En este modelo, la decisión sobre qué operación realizar está principalmente definida de antemano por el código de la aplicación.

			Por ejemplo, si el desarrollador programa:

				GET /usuarios/123

		la aplicación realizará esa operación cuando se cumpla la condición programada. La API proporciona el contrato y las capacidades, pero normalmente es el código de la 			aplicación el que determina qué endpoint llamar, cuándo llamarlo y cómo interpretar su respuesta.

			Por eso podemos resumir una API como:

				Un contrato que permite que diferentes programas se comuniquen mediante operaciones y formatos previamente definidos.

			Es importante señalar que una API no necesariamente significa que exista HTTP. También pueden existir APIs de bibliotecas, sistemas operativos, bases de datos y 			otros tipos de software. Sin embargo, las APIs web son uno de los ejemplos más comunes.
	3.2 ¿Qué es MCP?

		MCP (Model Context Protocol) es un protocolo abierto diseñado para conectar aplicaciones de inteligencia artificial con fuentes de datos, herramientas y otros sistemas 		externos.

		Una diferencia fundamental con el consumo tradicional de una API es que MCP proporciona un mecanismo estandarizado para que un servidor publique las capacidades que ofrece, 		mientras que un cliente MCP puede descubrir esas capacidades y utilizarlas.

		Los servidores MCP pueden exponer diferentes tipos de primitivas, entre ellas:

			Tools (herramientas): acciones que el modelo puede solicitar.
			Resources (recursos): información que una aplicación puede incorporar al contexto del modelo.
			Prompts (plantillas de prompts): plantillas reutilizables que puede seleccionar el usuario.

		La documentación del SDK oficial de MCP explica que las herramientas tienen un nombre, descripción y esquema de entrada, y que el cliente puede obtener esa información 		mediante operaciones como tools/list. Posteriormente puede solicitar la ejecución de una herramienta mediante tools/call.

			De forma simplificada:

				Usuario ---> LLM ---"Necesito buscar..."---> Cliente MCP ---descubre tools---> Servidor MCP ---tools/list---> nombre, descripción,parámetros / esquema

		Una vez que el modelo dispone de las herramientas disponibles, puede determinar cuál utilizar de acuerdo con la tarea solicitada por el usuario.

			Por ejemplo, si un servidor publica:

				buscar_archivo
			Descripción: Busca archivos dentro de los directorios autorizados.

				leer_archivo
			Descripción: Lee el contenido de un archivo autorizado.

				crear_archivo
			Descripción: Crea un nuevo archivo.

		el modelo puede determinar cuál herramienta corresponde a la solicitud del usuario.

		Esta característica es una diferencia importante respecto al modelo tradicional de una API: las capacidades pueden descubrirse durante la ejecución en lugar de que todas 		las operaciones concretas tengan que estar codificadas previamente en la aplicación.

	3.3 MCP utiliza JSON-RPC 2.0

		MCP utiliza JSON-RPC como parte de su comunicación entre clientes y servidores. Esto proporciona una estructura estandarizada para enviar solicitudes, respuestas y 			notificaciones.

			Por ejemplo, una llamada MCP puede tener una estructura como:

				{
				  "jsonrpc": "2.0",
				  "id": 1,
				  "method": "tools/call",
				  "params": {
				    "name": "buscar_archivo",
				    "arguments": {
				      "nombre": "documento.txt"
				    }
				  }
				}

			En este ejemplo:

				"jsonrpc": "2.0" indica el formato de comunicación.
				"id": 1 identifica la solicitud.
				"method": "tools/call" indica que se solicita la ejecución de una herramienta.
				"name" identifica la herramienta.
				"arguments" contiene los parámetros.

		La especificación actual de MCP documenta las operaciones de herramientas y utiliza mensajes JSON-RPC para la comunicación entre las partes.

		Esto no significa que MCP sea simplemente una API HTTP. JSON-RPC es parte del protocolo de comunicación de MCP, mientras que MCP define además cómo se descubren y utilizan 		capacidades como herramientas, recursos y prompts.

	3.4 Diferencia fundamental: ¿quién decide qué se invoca?

		Esta es probablemente la diferencia más importante que debemos explicar en el trabajo.

			En una API tradicional:

				Desarrollador ----> Escribe código ----Decide qué endpoint llamar----> API

		La aplicación ya contiene la lógica que determina qué operación realizar.

			En MCP:

				Usuario ---> Solicitud ---> Modelo ---> Descubre herramientas disponibles ---> Decide qué herramienta es adecuada ---> Cliente MCP ---> Servidor MCP

		Aquí el modelo puede seleccionar una herramienta del catálogo disponible según la tarea.

		La documentación oficial distingue precisamente las herramientas de otras primitivas porque las herramientas son las que el modelo puede decidir llamar, mientras que los 		recursos son controlados por la aplicación y los prompts son seleccionados por el usuario.

		Esto no significa que el modelo tenga libertad ilimitada. El cliente y el servidor siguen estableciendo qué herramientas existen, qué parámetros aceptan y qué permisos o 		controles se aplican.

	3.5 Tabla comparativa entre MCP y una API

			============================================================
			        COMPARACIÓN: API TRADICIONAL vs MCP
			============================================================

			CARACTERÍSTICA: ¿Quién decide qué se invoca?

			API TRADICIONAL:
			Normalmente el código de la aplicación, escrito previamente por el desarrollador.

			MCP:
			El modelo puede seleccionar una herramienta disponible según la solicitud del usuario, dentro de las capacidades y controles del cliente.

			------------------------------------------------------------

			CARACTERÍSTICA: ¿Cómo se descubren las capacidades?

			API TRADICIONAL:
			El desarrollador consulta la documentación de la API y programa las operaciones que necesita.

			MCP:
			El cliente puede descubrir las herramientas, recursos y prompts que ofrece el servidor mediante el protocolo.

			------------------------------------------------------------

			CARACTERÍSTICA: Acoplamiento cliente-servicio

			API TRADICIONAL:
			Puede existir un acoplamiento importante: el código debe conocer endpoints, parámetros y formatos específicos.

			MCP:
			El protocolo estandariza la forma de descubrir e invocar capacidades, permitiendo que distintos clientes trabajen con servidores MCP compatibles.

			------------------------------------------------------------

			CARACTERÍSTICA: Formato de mensajes

			API TRADICIONAL:
			Depende de la API. Puede utilizar JSON/HTTP, XML, GraphQL, formatos binarios u otros mecanismos.

			MCP:
			MCP utiliza mensajes estructurados basados en JSON-RPC y define operaciones específicas para sus primitivas.

			------------------------------------------------------------

			CARACTERÍSTICA: Autenticación y consentimiento

			API TRADICIONAL:
			Dependen de la API y de la aplicación: API keys, OAuth,tokens, sesiones, permisos, etc.

			MCP:
			MCP contempla mecanismos de autorización y el cliente puede aplicar controles y consentimiento según el entorno.
			El protocolo no significa que todas las acciones sean automáticamente autorizadas.

			------------------------------------------------------------

			CARACTERÍSTICA: Reutilización

			API TRADICIONAL:
			Una API puede reutilizarse en distintas aplicaciones, pero cada aplicación debe implementar cómo consumirla.

			MCP:
			Un servidor MCP puede ser utilizado por diferentes clientes compatibles con MCP, que pueden descubrir sus capacidades mediante el protocolo.

			------------------------------------------------------------

			CARACTERÍSTICA: Descubrimiento de herramientas

			API TRADICIONAL:
			No es necesariamente una característica de la API; normalmente el desarrollador conoce las operaciones por documentación.

			MCP:
			Es una característica central del protocolo: las capacidades pueden anunciarse y descubrirse mediante el protocolo.

			------------------------------------------------------------

			CARACTERÍSTICA: Objetivo principal

			API TRADICIONAL:
			Permitir la comunicación entre programas y ofrecer operaciones o datos.

			MCP:
			Estandarizar la conexión entre aplicaciones de IA y herramientas, datos y recursos externos.

			============================================================

		La comparación anterior se basa en la documentación de MCP y en la definición general de las APIs. La documentación oficial de MCP muestra, por ejemplo, que un cliente 		puede consultar tools/list, obtener nombre, descripción y esquema de entrada, y posteriormente utilizar tools/call.

	3.6 Autenticación y consentimiento

		Es importante no caer en la idea de que MCP elimina la necesidad de autenticación o permisos.

		Una API puede utilizar diferentes mecanismos de autenticación, como claves, tokens u OAuth. El método concreto depende del servicio.

		MCP también contempla mecanismos para proteger las conexiones y controlar el acceso. Además, el cliente puede aplicar políticas de consentimiento para determinadas 			acciones.

		Esto es especialmente importante cuando las herramientas tienen efectos secundarios. Por ejemplo, leer un archivo puede ser una operación diferente a eliminarlo o 			modificarlo.

		Por eso, un sistema MCP correctamente implementado puede establecer controles como:

		Modelo-->Solicita herramienta-->Cliente verifica(¿La herramienta está permitida?,¿Los parámetros son válidos?,¿Se necesita autorización?)-->Servidor MCP-->Ejecuta operación 

		La existencia de MCP no significa que cualquier modelo pueda ejecutar cualquier acción automáticamente. Los permisos y mecanismos de autorización continúan siendo 			responsabilidad del sistema que implementa la conexión.
	
	3.7 ¿MCP sustituye a las APIs?

		No. MCP no sustituye ni vuelve obsoletas a las APIs.

		Esta aclaración es fundamental para esta actividad.

		MCP y las APIs cumplen funciones diferentes y pueden utilizarse conjuntamente.

		Una API puede proporcionar un servicio como:

			API de clima
			API de pagos
			API de una base de datos
			API de almacenamiento

		Un servidor MCP puede actuar como una capa de integración que presenta determinadas capacidades de ese servicio mediante herramientas que un cliente compatible con MCP 		puede descubrir y utilizar.

			Por ejemplo:

              			Aplicación de IA --> Cliente MCP --> Servidor MCP --> API existente --> Servicio externo

		Supongamos que una empresa ya tiene una API para consultar información de sus clientes. No necesariamente tendría que reemplazar esa API para utilizar MCP. Podría crear un 		servidor MCP que exponga una herramienta como:

			buscar_cliente

		El servidor MCP recibiría la solicitud y, internamente, podría realizar una llamada a la API existente.

			Por lo tanto:

				MCP puede funcionar como una capa por encima de APIs y otros recursos existentes, proporcionando una forma estandarizada y descubrible de poner sus 					capacidades a disposición de aplicaciones de IA.

		La documentación oficial describe precisamente a MCP como un estándar abierto para conectar aplicaciones de IA con los sistemas donde se encuentran los datos y las 			herramientas.

	3.8 Ejemplo sencillo: API frente a MCP

		Para visualizar la diferencia, podemos imaginar un sistema de archivos.

		Utilizando una API

			Un desarrollador podría tener una API con endpoints como:

				GET    /files
				GET    /files/{id}
				POST   /files
				PUT    /files/{id}
				DELETE /files/{id}

		El desarrollador estudia esa documentación y escribe el código necesario para llamar a cada endpoint.

			Desarrollador
			      │
			      ▼
			Escribe código
			      │
   			   ├── GET /files
   			   ├── GET /files/{id}
   			   └── POST /files
     			         │
      			        ▼
       			      API
			Utilizando MCP

		Un servidor MCP podría publicar herramientas como:

			listar_archivos
			leer_archivo
			crear_archivo
			modificar_archivo
			buscar_archivo

		El cliente puede descubrir estas herramientas y proporcionar sus descripciones y esquemas al modelo.

			Usuario:
					"Busca el archivo donde aparece mi configuración."
      					      --> LLM
  						--> descubre herramientas
		     				 --> buscar_archivo
		      				  --> Servidor MCP
  			  		           -->Sistema de archivos

		La diferencia fundamental es que la API proporciona una interfaz que el código debe consumir, mientras que MCP proporciona un protocolo estandarizado mediante el cual las 		capacidades pueden ser descubiertas y utilizadas por un sistema de IA.

REFERENCIAS
	IBM. (s. f.). ¿Qué es una API? IBM. https://www.ibm.com/es-es/think/topics/api
	MDN Web Docs. (s. f.). Web APIs. Mozilla. https://developer.mozilla.org/en-US/docs/Web/API
	Model Context Protocol. (2026). The 2026-07-28 Specification. https://blog.modelcontextprotocol.io/posts/2026-07-28/
	Model Context Protocol. (2026). MCP Python SDK — Primeros pasos. https://py.sdk.modelcontextprotocol.io/es/get-started/first-steps/
	Model Context Protocol. (2026). MCP TypeScript SDK. https://ts.sdk.modelcontextprotocol.io/v2/
	Model Context Protocol. (2026). Authorization. https://modelcontextprotocol.io/specification/2025-11-25/basic/authorization
	Model Context Protocol. (2026). Security best practices. https://modelcontextprotocol.io/specification/draft/basic/security_best_practices
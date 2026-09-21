4. Arquitectura de MCP
	4.1 Modelo Host / Cliente / Servidor

		La arquitectura de Model Context Protocol (MCP) se organiza alrededor de tres componentes principales: host, cliente y servidor.

		Estos componentes cumplen funciones diferentes y no deben confundirse.

			Host

				El host es la aplicación que proporciona el entorno principal donde interactúa el usuario con el modelo de inteligencia artificial.

				Por ejemplo, puede ser un entorno de desarrollo, una aplicación de escritorio o cualquier otra aplicación compatible con MCP.

				El host es responsable de administrar la interacción general con el usuario y normalmente contiene uno o varios clientes MCP.

			Cliente

				El cliente MCP es el componente que mantiene la comunicación entre el host y un servidor MCP.

				El cliente conoce el protocolo MCP y se encarga de comunicarse con los servidores, descubrir sus capacidades y solicitar operaciones.

				Una aplicación host puede tener diferentes clientes para conectarse con diferentes servidores:

								                    HOST
								        +--------------------------+
								        |                          |
								        |    Aplicacion de IA      |
								        |                          |
								        |    +---------------+     |
								        |    |   Cliente MCP |     |
								        |    +-------+-------+     |
								        |            |             |
								        +------------|-------------+
								                     |
								                     v
								              +--------------+
								              | Servidor MCP |
								              +--------------+
			Servidor

				El servidor MCP es el componente que proporciona capacidades al cliente.

				Puede ofrecer herramientas, recursos y plantillas de prompts. El servidor puede conectarse a diferentes sistemas externos, como archivos, bases de datos, 				APIs o servicios especializados.

				En nuestra implementación utilizaremos un servidor MCP de sistema de archivos, cuyo objetivo será proporcionar herramientas para trabajar con archivos 					dentro de los directorios que autoricemos.

				Por lo tanto, en nuestra práctica podemos identificar los roles de esta manera:

			HOST
				Cliente MCP
				    │
				    ▼
				Servidor MCP Filesystem
				    │
				    ▼
				Directorio autorizado
				    │
				    ├── archivo1.txt
				    ├── archivo2.txt
				    └── carpeta/

		Es importante aclarar que el modelo no se convierte en el servidor ni accede directamente al disco. El modelo interactúa con el sistema a través del cliente y del servidor 		MCP.

		La documentación oficial describe esta arquitectura cliente-servidor como la base de MCP.

	4.2 Primitivas que expone un servidor

		Un servidor MCP puede proporcionar diferentes tipos de capacidades. Las principales primitivas del servidor son tools, resources y prompts.

			Tools — Herramientas

				Las tools (herramientas) permiten que el modelo solicite que se realice una acción.

			Por ejemplo, un servidor de archivos puede proporcionar herramientas como:

				listar_directorio
				leer_archivo
				crear_archivo
				escribir_archivo
				mover_archivo
				buscar_archivo

			Cada herramienta tiene información que permite conocer:

				su nombre;
				su descripción;
				los parámetros que acepta;
				el esquema de entrada;
				y, en las versiones modernas, puede definir también un esquema de salida.

		El cliente puede descubrir las herramientas mediante operaciones de listado y posteriormente solicitar la ejecución de una herramienta específica.

			Por ejemplo:
				tools/list ----> leer_archivo , crear_archivo , buscar_archivo , mover_archivo   

		Las herramientas son especialmente importantes para nuestra práctica porque el servidor Filesystem utilizará tools para permitir operaciones sobre los archivos autorizados.

		Resources — Recursos

			Los resources (recursos) representan información que puede ponerse a disposición de la aplicación de IA.

			Un recurso puede identificarse mediante un URI y puede representar información procedente de diferentes fuentes.

				Por ejemplo, un servidor podría ofrecer:

					file:///proyecto/documentacion.md

				como un recurso que el cliente puede consultar.

			Una diferencia importante es que resources no son lo mismo que tools.

			Una herramienta representa normalmente una acción que puede ejecutarse, mientras que un recurso representa información que puede incorporarse al contexto.

				Por ejemplo:

					TOOL
						Busca el archivo configuración.json" ----> Acción

					RESOURCE
						"file:///configuracion.json" ----> Información

		Esta distinción es importante porque MCP no consiste únicamente en una colección de funciones que el modelo puede ejecutar. El protocolo también define mecanismos para 		trabajar con información contextual.

			Prompts — Plantillas de prompt

				Los prompts son plantillas reutilizables que un servidor puede proporcionar para facilitar determinadas interacciones.

				Un servidor puede ofrecer una plantilla con determinados argumentos que el usuario puede seleccionar o completar.

			Por ejemplo:

				prompt: revisar_codigo

				Argumentos:
					- lenguaje
					- archivo
					- objetivo

		El cliente puede utilizar esa plantilla para construir una interacción determinada con el modelo.

			Por lo tanto:

				TOOLS      → acciones
				RESOURCES  → información
				PROMPTS    → plantillas de interacción

		Las tres primitivas forman parte de la arquitectura de MCP y permiten que un servidor proporcione mucho más que simples llamadas a funciones.


	4.3 Primitivas del lado del cliente: Roots y Elicitation

		Además de las capacidades que proporciona el servidor, MCP también contempla capacidades del lado del cliente.

		Entre ellas se encuentran roots y elicitation.

			Roots

				Los roots fueron diseñados para que el cliente pueda indicar al servidor cuáles son determinados límites de acceso a recursos del sistema de archivos.

					Por ejemplo, un cliente podría indicar que una determinada carpeta constituye una raíz disponible:

						C:\MCP-Practica\

					Esto permite que el servidor conozca el espacio que el cliente está poniendo a disposición.

				Sin embargo, hay una actualización importante que debemos documentar correctamente: roots está marcado como obsoleto a partir de la especificación 					2026-07-28. En esa versión, la documentación recomienda utilizar alternativas como parámetros de herramientas, URIs de recursos, configuración del servidor 				o variables de entorno.

			Por lo tanto, para nuestro trabajo podemos explicar roots porque la consigna explícitamente pide mencionarlo, pero debemos indicar su estado actual:

		En versiones anteriores de MCP, roots permitía que el cliente proporcionara al servidor límites de archivos o directorios disponibles. En la especificación 2026-07-28 esta 		primitiva está marcada como obsoleta y se recomiendan mecanismos alternativos para definir el alcance.

		Esto demuestra que estamos consultando una versión concreta de la especificación en lugar de presentar información antigua como si fuera actual.

			Elicitation

				La elicitation permite que un servidor solicite información adicional al usuario a través del cliente.

				Por ejemplo, imaginemos que una herramienta necesita un dato que no fue proporcionado originalmente:

					--> Usuario: Realiza la operación. 
						--> Servidor MCP: Necesito que confirmes X. 
							--> Cliente: MCP solicita confirmación:¿Deseas continuar?
								--> Usuario: Sí.
									--> Servidor continúa

		Esto resulta especialmente útil para acciones que requieren información adicional o confirmación del usuario.

		En la especificación 2026-07-28, el modelo de comunicación cambió para permitir este tipo de interacción mediante Multi Round-Trip Requests (MRTR). En lugar de depender 		necesariamente de una conexión mantenida abierta, el servidor puede devolver que necesita información adicional y el cliente puede continuar la operación proporcionando la 		respuesta.

	4.4 Transportes de MCP

		El transporte determina cómo se intercambian los mensajes entre el cliente y el servidor MCP.

		La especificación y los SDK actuales contemplan principalmente dos formas de transporte:

			stdio
			Streamable HTTP
			stdio

		stdio significa standard input/output.

		Este transporte está pensado principalmente para servidores MCP locales.

		En este escenario, el cliente inicia el servidor como un proceso hijo y ambos se comunican mediante:

			Cliente MCP ----stdin----> Servidor MCP ----stdout----> Cliente MCP

		La comunicación utiliza mensajes MCP sobre la entrada y salida estándar del proceso.

			Por ejemplo, en nuestra futura implementación local, el cliente puede iniciar el servidor Filesystem como un proceso y comunicarse con él mediante stdin y stdout.

		Una ventaja de stdio es que no necesitamos levantar un servidor web ni abrir un puerto HTTP para una integración local.

		El SDK oficial de TypeScript describe StdioServerTransport precisamente para integraciones locales en las que el cliente inicia el servidor como proceso hijo.

			Streamable HTTP

				Streamable HTTP es el transporte destinado principalmente a escenarios donde el servidor MCP se encuentra disponible mediante HTTP.

			En lugar de iniciar el servidor como un proceso hijo, el cliente se comunica con un endpoint HTTP.

		De forma simplificada:

			Cliente MCP ----HTTP----> Servidor MCP (remoto)

		Streamable HTTP permite realizar solicitudes mediante HTTP y puede utilizar mecanismos de streaming cuando son necesarios.

		La documentación actual del SDK de MCP lo describe como el transporte moderno y completo para servidores HTTP.

		Es importante no confundirlo con el antiguo transporte HTTP + SSE. Streamable HTTP es el transporte moderno, mientras que HTTP + SSE se mantiene principalmente por 			compatibilidad con implementaciones anteriores.
	
	4.5 Diferencia entre stdio y Streamable HTTP

		Podemos resumir la diferencia de esta manera:

			===============================================================
			              COMPARACIÓN: STDIO vs STREAMABLE HTTP
			================================================================

			CARACTERÍSTICA: Uso principal

			STDIO:
			Servidores locales.

			STREAMABLE HTTP:
			Servidores remotos o accesibles mediante HTTP.

			----------------------------------------------------------------

			CARACTERÍSTICA: Proceso

			STDIO:
			El cliente puede iniciar el servidor como proceso hijo.

			STREAMABLE HTTP:
			El servidor funciona como servicio HTTP.

			----------------------------------------------------------------

			CARACTERÍSTICA: Comunicación

			STDIO:
			stdin / stdout

			STREAMABLE HTTP:
			HTTP

			----------------------------------------------------------------

			CARACTERÍSTICA: Red necesaria

			STDIO:
			No necesariamente.

			STREAMABLE HTTP:
			Sí, para comunicación HTTP.

			----------------------------------------------------------------

			CARACTERÍSTICA: Puertos

			STDIO:
			No necesita un puerto HTTP.

			STREAMABLE HTTP:
			Utiliza un endpoint HTTP.

			----------------------------------------------------------------

			CARACTERÍSTICA: Ejemplo

			STDIO:
			Servidor Filesystem local.

			STREAMABLE HTTP:
			Servidor MCP alojado en un servidor remoto.

			----------------------------------------------------------------

			CARACTERÍSTICA: Escenario típico

			STDIO:
			IDE o aplicación de escritorio.

			STREAMABLE HTTP:
			Servicio compartido o infraestructura remota.

			================================================================

		El SDK oficial de MCP documenta ambos transportes y señala específicamente que stdio está destinado a integraciones locales donde el cliente inicia el servidor como proceso 		hijo, mientras que Streamable HTTP es el transporte moderno para servidores HTTP.

	4.6 Arquitectura de nuestra implementación

		Para nuestra práctica utilizaremos un servidor MCP de sistema de archivos local.

		Por lo tanto, la arquitectura será aproximadamente:

				+---------------------------------------+
				|                 HOST                  |
				|                                       |
				|             Cliente MCP               |
				|                                       |
				|       Aplicación que utilizamos       |
				+-------------------+-------------------+
				                    |
				                    | stdio
				                    |
				                    v
				+---------------------------------------+
				|        SERVIDOR MCP FILESYSTEM        |
				|                                       |
				|  Tools:                               |
				|    - listar                           |
				|    - leer                             |
				|    - escribir                         |
				|    - crear                            |
				|    - mover                            |
				|    - buscar                           |
				+-------------------+-------------------+
				                    |
				                    |
				                    v
				+---------------------------------------+
				|          DIRECTORIO AUTORIZADO        |
				|                                       |
				|  C:\MCP-Practica\                     |
				|      +-- archivo1.txt                 |
				|      +-- archivo2.txt                 |
				|      +-- documentos\                  |
				+---------------------------------------+


		En este caso:

			Host: la aplicación compatible con MCP que utilicemos.
			Cliente: el componente MCP integrado en ese host.
			Servidor: el servidor MCP Filesystem.
			Transporte: stdio, porque será una integración local.
			Recurso protegido: nuestro directorio de trabajo específico.
			Tools: las operaciones que el servidor proporciona para trabajar con archivos.

		Esta separación será particularmente importante durante la prueba de seguridad, porque podremos demostrar que el servidor tiene acceso únicamente al directorio que le 			hayamos autorizado.

	4.7 Versión de la especificación consultada

		Para esta investigación se consultó la especificación:

			Model Context Protocol Specification 2026-07-28

			Fecha de publicación: 28 de julio de 2026.

		Esta versión es importante porque MCP se actualiza periódicamente. La versión 2026-07-28 introdujo cambios importantes, entre ellos un núcleo de protocolo sin estado, Multi 		Round-Trip Requests (MRTR), cambios en Streamable HTTP y modificaciones en algunas primitivas.

		En particular, esta versión marca roots como obsoleto, por lo que en este documento se menciona por ser una primitiva relevante de versiones anteriores y por estar incluida 		en la consigna de la actividad, pero se aclara su estado actual.

			Por lo tanto, la versión de referencia de este documento es:

				Protocolo: Model Context Protocol (MCP)
				Versión: 2026-07-28
				Fecha: 28 de julio de 2026

		Esto debe aparecer también en nuestro README para que quede claro sobre qué versión se realizó la investigación.

REFERENCIAS
	Model Context Protocol. (2026, 28 de julio). The 2026-07-28 Specification. Model Context Protocol. https://blog.modelcontextprotocol.io/posts/2026-07-28/
	Model Context Protocol. (2026). MCP TypeScript SDK — Server. https://ts.sdk.modelcontextprotocol.io/server
	Model Context Protocol. (2026). Roots. MCP Ruby SDK. https://ruby.sdk.modelcontextprotocol.io/server/roots/
	Model Context Protocol. (2026). Protocol versions. MCP TypeScript SDK. https://ts.sdk.modelcontextprotocol.io/v2/protocol-versions/
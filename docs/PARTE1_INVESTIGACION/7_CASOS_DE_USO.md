7. Casos de uso
	7.1 ¿Qué significa que una herramienta implemente MCP?
		Model Context Protocol (MCP) es un protocolo abierto que permite conectar aplicaciones de inteligencia artificial con herramientas, datos y servicios externos.

		Una herramienta que implementa MCP puede actuar como cliente MCP, permitiendo conectar servidores que proporcionan diferentes capacidades.

		Esto permite pasar de una interacción limitada a copiar y pegar información hacia un modelo a una interacción en la que la aplicación puede obtener información o ejecutar 		herramientas directamente sobre el entorno de desarrollo.

			Por ejemplo:
				Antes:
					Usuario
						--> Copia código
							--> Copia archivos
								--> Pega información --> LLM

			Con una aplicación compatible con MCP:

				Usuario
					--> Aplicación de desarrollo
						--> Cliente MCP
							--> Servidor MCP (Archivos , Base de datos , API , Otros servicios)

		De esta manera, el modelo puede trabajar con información proporcionada por herramientas conectadas a la aplicación.

		Anthropic describe MCP precisamente como un protocolo abierto para estandarizar la conexión entre aplicaciones de IA, fuentes de datos y herramientas.

	7.2 Google Antigravity

		Google Antigravity es un entorno de desarrollo orientado a agentes de IA que permite trabajar con proyectos de software y herramientas externas.

		La documentación oficial describe Antigravity como un entorno de desarrollo con agentes que pueden razonar sobre código existente y utilizar diferentes herramientas. Entre 		las capacidades de personalización se encuentra explícitamente MCP.

		Además, la documentación oficial de Antigravity tiene una sección específica dedicada a MCP y explica que permite conectar los agentes con herramientas locales, bases de 		datos, analizadores de archivos y APIs remotas.

			¿Para qué utiliza MCP?

				Antigravity puede utilizar servidores MCP para proporcionar al agente acceso a herramientas y fuentes de información que no forman parte directamente de su 				contexto inicial.

					Por ejemplo:

									                    GOOGLE ANTIGRAVITY
									                            |
									                            v
									                        AGENTE IA
									                            |
									                            v
									                       CLIENTE MCP
									                            |
									                  +---------+---------+
									                  |                   |
									                  v                   v
									          SERVIDOR MCP         SERVIDOR MCP
								                  DE ARCHIVOS        DE BASE DE DATOS
									                  |                   |
									                  v                   v
									              ARCHIVOS           INFORMACIÓN
									            DEL PROYECTO          DE LA BD

		La documentación de Antigravity señala que MCP permite obtener contexto directamente y ejecutar acciones mediante herramientas conectadas.

		También existe un sistema de permisos para MCP. De manera predeterminada, las herramientas MCP sin configurar requieren aprobación antes de ejecutarse, y las políticas 		permiten controlar herramientas individuales o servidores completos.

		Esto resulta especialmente relevante para nuestra investigación porque demuestra que conectar una herramienta mediante MCP no significa otorgarle automáticamente permisos 		ilimitados.

	7.3 Qwen Code

		Aquí es importante hacer una aclaración.

		Qwen no es una plataforma de desarrollo.

		Qwen es una familia de modelos de inteligencia artificial. Para este punto de la investigación debemos referirnos específicamente a Qwen Code, que es una herramienta de 		línea de comandos para desarrollo que puede conectarse a servidores MCP.

		La documentación oficial de Qwen Code indica explícitamente que puede conectarse a herramientas y fuentes de datos externas mediante MCP.

			Por lo tanto, la forma correcta de escribirlo es:

				Qwen Code implementa soporte para MCP.

						Y no:

								"Qwen es una plataforma MCP."

		7.3.1 ¿Para qué utiliza MCP Qwen Code?

			Qwen Code puede utilizar servidores MCP para ampliar las capacidades disponibles para el modelo.

				La documentación oficial menciona, entre otros usos:

						trabajar con archivos y repositorios;
						consultar bases de datos;
						integrar servicios internos;
						utilizar APIs mediante servidores MCP;
						automatizar flujos de trabajo.

				Podemos representarlo así:
									
									                         QWEN CODE
									                            |
									                            v
									                          MODELO
									                            |
									                            v
									                       CLIENTE MCP
									                            |
									             +--------------+--------------+
									             |              |              |
									             v              v              v
									          ARCHIVOS       BASE DE        SERVICIOS
									          Y REPOS        DATOS          EXTERNOS

				Por ejemplo, un servidor MCP podría proporcionar herramientas para leer y modificar archivos de un proyecto.

		Qwen Code puede descubrir las herramientas proporcionadas por el servidor y utilizarlas según la tarea solicitada. La documentación explica que los servidores MCP permiten 		descubrir herramientas, sus descripciones y sus parámetros mediante definiciones estandarizadas.

	7.4 Cursor

		Cursor es un entorno de desarrollo con capacidades de IA que también implementa MCP.

		Su documentación oficial explica que MCP permite conectar Cursor con herramientas y fuentes de datos externas. Cursor puede utilizar servidores MCP locales o remotos.

		Por ejemplo:

                         CURSOR
                            |
                            v
                     AGENTE DE IA
                            |
                  +---------+---------+
                  |                   |
                  v                   v
            HERRAMIENTAS             MCP
              INTERNAS             SERVERS
                  |                   |
                  v                   v
              CÓDIGO             SERVICIOS
               LOCAL             EXTERNOS

		Cursor permite configurar servidores MCP mediante archivos como:

				.cursor/mcp.json

			para un proyecto específico, o mediante una configuración global.

		También permite habilitar o deshabilitar herramientas MCP desde la interfaz y, de manera predeterminada, solicita aprobación antes de utilizar herramientas MCP.

	7.4.1 ¿Cómo trabaja Cursor con un repositorio?

		Cursor dispone de herramientas propias para explorar y modificar un código existente.

		Su modo Agent puede explorar el código, editar múltiples archivos, ejecutar comandos y corregir errores durante una tarea.

			Por ejemplo, si tenemos:

				MiProyecto/
				│
				├── src/
				│   ├── main.py
				│   ├── usuario.py
				│   └── base.py
				│
				├── tests/
				│   └── test_usuario.py
				│
				└── README.md

			podemos solicitar una tarea relacionada con el proyecto.

		El agente puede explorar el código existente, localizar archivos relevantes y realizar modificaciones en diferentes archivos.

			Esto es diferente de tener que copiar manualmente:

				main.py
				usuario.py
				base.py
				test_usuario.py
				README.md

			y pegarlos uno por uno dentro de una conversación.

		Cursor proporciona herramientas de búsqueda, lectura y edición sobre el código del proyecto, y MCP añade la posibilidad de conectar servicios externos mediante un protocolo 		estandarizado.

	7.5 Comparación de los tres casos

		Podemos resumir los tres ejemplos de esta manera:
				
				================================================================================================
				                         COMPARACIÓN DE HERRAMIENTAS MCP
				================================================================================================

				+--------------------+------------------------+------------------------+------------------------+
				| Herramienta        | Tipo                   | Uso de MCP             | Ejemplo de capacidad   |
				+--------------------+------------------------+------------------------+------------------------+
				| Google Antigravity | Entorno de desarrollo  | Conectar agentes con   | Archivos, bases de     |
				|                    | agéntico               | herramientas y fuentes | datos, APIs y otros    |
				|                    |                        | externas               | servicios              |
				+--------------------+------------------------+------------------------+------------------------+
				| Qwen Code          | Herramienta CLI para   | Conectar herramientas  | Archivos, repositorios |
				|                    | desarrollo             | y datos externos       | bases de datos y APIs  |
				+--------------------+------------------------+------------------------+------------------------+
				| Cursor             | Entorno de desarrollo  | Conectar herramientas  | Servicios, bases de    |
				|                    | con IA                 | y fuentes de datos     | datos y herramientas   |
				|                    |                        | externas               | MCP                    |
				+--------------------+------------------------+------------------------+------------------------+


		Las tres herramientas tienen implementaciones documentadas de MCP, aunque sus arquitecturas y características concretas son diferentes.

	7.6 ¿Cómo pueden trabajar con repositorios completos?

		Esta parte es especialmente importante para entender por qué MCP cambia la interacción con los entornos de desarrollo.

		En un flujo tradicional utilizando solamente un chat web, el usuario podría tener que proporcionar manualmente el contenido:

				Usuario
				  │
				  ├── Copiar archivo 1
				  ├── Copiar archivo 2
				  ├── Copiar archivo 3
				  └── Copiar archivo 4
				          │
				          v
				         LLM

		Esto presenta varios problemas:

			requiere trabajo manual;
			puede omitir archivos importantes;
			puede perderse información sobre la estructura del proyecto;
			los archivos grandes pueden superar los límites de contexto;
			el modelo no tiene necesariamente acceso al estado actual del proyecto.

		En un entorno de desarrollo con herramientas de acceso al proyecto:

                         PROYECTO LOCAL
                               |
                               v
                      HERRAMIENTAS DEL IDE
                               |
                               v
                         AGENTE DE IA
                               |
                    +----------+----------+
                    |                     |
                    v                     v
              LEER ARCHIVOS        BUSCAR ARCHIVOS
                    |                     |
                    +----------+----------+
                               |
                               v
                           ANALIZAR
                               |
                               v
                       EDITAR ARCHIVOS
                               |
                               v
                         PROYECTO LOCAL

		El usuario no necesita subir manualmente cada archivo porque la aplicación ya se encuentra trabajando dentro del entorno del proyecto y puede utilizar herramientas para 		localizar, leer y modificar los archivos necesarios.

	7.7 MCP y el acceso al repositorio

		Aquí debemos hacer una distinción importante.

		MCP no significa automáticamente "acceso a todo el repositorio".

			El acceso depende de:

				qué cliente se está utilizando;
				qué servidores MCP están configurados;
				qué herramientas proporciona cada servidor;
				qué directorios o recursos están autorizados;
				qué permisos tiene la aplicación;
				qué acciones requieren aprobación.

		Por ejemplo:

                         REPOSITORIO
                              |
                              v
                  APLICACIÓN DE DESARROLLO
                              |
                 +------------+------------+
                 |                         |
                 v                         v
        HERRAMIENTAS INTERNAS        SERVIDORES MCP
                                            |
                                            v
                                  HERRAMIENTAS DISPONIBLES
                                            |
                                            v
                                    RECURSOS AUTORIZADOS

		En Cursor, por ejemplo, las herramientas MCP pueden activarse o desactivarse y el sistema solicita aprobación antes de utilizarlas por defecto.

		En Antigravity también existe un sistema de permisos específico para las herramientas MCP.

		Qwen Code, por su parte, permite configurar servidores MCP a nivel de usuario o de proyecto y permite controlar qué herramientas se incluyen o excluyen.

		Esto conecta directamente con lo explicado en el punto 6 de Seguridad: el objetivo no es simplemente permitir que el modelo haga cosas, sino controlar qué capacidades tiene 		y sobre qué recursos puede utilizarlas.

	7.8 Ejemplo práctico de modificación de un repositorio

		Imaginemos un repositorio:

			mi-proyecto/
			│
			├── src/
			│   ├── usuarios.py
			│   ├── autenticacion.py
			│   └── base_datos.py
			│
			├── tests/
			│   ├── test_usuarios.py
			│   └── test_auth.py
			│
			├── config/
			│   └── config.json
			│
			└── README.md

		El usuario podría solicitar:

			"Agrega validación de correo electrónico al registro de usuarios y actualiza las pruebas."

			El agente puede necesitar realizar varios pasos:

				1. Buscar dónde se registra un usuario
				2. Leer usuarios.py
				3. Leer las pruebas relacionadas
				4. Identificar cómo funciona actualmente
				5. Modificar usuarios.py
				6. Modificar test_usuarios.py
				7. Ejecutar las pruebas
				8. Revisar resultados
				9. Corregir errores si existen

		La ventaja del entorno agéntico es que la persona no tiene que copiar manualmente cada archivo al modelo.

		El agente obtiene el contexto necesario mediante las herramientas disponibles.

		En Cursor, por ejemplo, su modo Agent está diseñado para explorar el código, realizar ediciones en múltiples archivos y ejecutar comandos como parte de una tarea.

				MCP amplía este modelo permitiendo que el entorno se conecte con servicios y herramientas externas, por ejemplo:

                         AGENTE
                           |
             +-------------+-------------+
             |             |             |
             v             v             v
          ARCHIVOS       GITHUB      BASE DE DATOS
             |             |             |
             +-------------+-------------+
                           |
                           v
                        PROYECTO

	7.9 ¿Qué cambia respecto a subir archivos manualmente?

		La principal diferencia puede resumirse de esta manera:

			INTERACCIÓN TRADICIONAL

				Usuario
				   │
				   ├── Selecciona archivo
				   ├── Sube archivo
				   ├── Explica contexto
				   ├── Sube otro archivo
				   └── Repite
				          │
				          v
				         LLM

		frente a:

			ENTORNO CON HERRAMIENTAS + MCP

								Usuario
								   │
								   v
						    "Modifica el sistema de usuarios"
								   │
								   v
								Agente
								   │
								   ├── Busca archivos relevantes
								   ├── Lee información
								   ├── Utiliza herramientas
								   ├── Modifica archivos
								   └── Comprueba resultados
 								         │
							                 v
								      Proyecto local

		Esto permite que la interacción sea más cercana a la forma en que trabaja una persona desarrolladora dentro de un entorno de programación.

		Sin embargo, MCP no es el responsable por sí solo de todas las capacidades de edición de código. Una aplicación como Cursor o Antigravity también posee herramientas propias 		para trabajar con archivos, código, terminales y proyectos. MCP proporciona una forma estandarizada de conectar capacidades adicionales. Cursor, por ejemplo, diferencia sus 		herramientas internas de las herramientas proporcionadas mediante MCP.

	7.10 Importancia de estos casos de uso

		Estos ejemplos muestran cómo MCP permite pasar de una interacción basada principalmente en texto a una interacción donde el modelo puede trabajar mediante herramientas 		conectadas al entorno de desarrollo.

			Podemos resumir la evolución de esta forma:
									
									+------------------------------------------------+
									|              CHAT TRADICIONAL                  |
									+------------------------------------------------+
									                       |
									                       v
									              USUARIO PROPORCIONA
									                 INFORMACIÓN
									                       |
									                       v
									                      LLM
									                       |
									                       v
									              RESPUESTA DE TEXTO
									                       |
									                       v
 									                 MCP + TOOLS
									                       |
									                       v
									+------------------------------------------------+
									|                  MCP + TOOLS                   |
									+------------------------------------------------+
									                       |
									                       v
									                    USUARIO
									                       |
									                       v
									                ENTORNO DE IA
 									                      |
 									                      v
 									                 CLIENTE MCP
 									                      |
 									            +---------+---------+
 									            |         |         |
 									            v         v         v
 									         ARCHIVOS    APIs    BASES DE DATOS
 									            |         |         |
 									            +---------+---------+
 									                      |
 									                      v
 									                     LLM
 									                      |
 									                      v
									    ACCIONES SOBRE EL ENTORNO AUTORIZADO

				La importancia de MCP no está en sustituir las herramientas existentes, sino en proporcionar un estándar común para conectar modelos y aplicaciones con 				herramientas y fuentes de información.

		Esto permite que diferentes aplicaciones puedan conectarse a servidores MCP sin que cada integración tenga que inventar necesariamente un mecanismo completamente diferente.

REFERENCIAS
	Anthropic. (s. f.). Model Context Protocol (MCP). Anthropic.https://docs.anthropic.com/en/docs/mcp
	Google. (2026). Model Context Protocol — Google Antigravity. Google Antigravity Documentation.https://antigravity.google/docs/mcp
	Google. (2026). Agent — Google Antigravity. Google Antigravity Documentation.https://antigravity.google/docs/agent
	Qwen Code. (2026). Connect Qwen Code to tools via MCP. Qwen Code Documentation.https://qwenlm.github.io/qwen-code-docs/en/users/features/mcp/
	Cursor. (s. f.). Model Context Protocol (MCP). Cursor Documentation.https://docs.cursor.com/context/model-context-protocol
	Cursor. (s. f.). Modes — Agent. Cursor Documentation.https://docs.cursor.com/agent
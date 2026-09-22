2\. Instalación del servidor de sistema de archivos



&#x09;2.1 Versiones utilizadas



&#x09;	La implementación se realizó utilizando:



&#x09;		- Sistema operativo: Windows

&#x09;		- Visual Studio Code: 1.138.0

&#x09;		- Node.js: v24.20.0

&#x09;		- Arquitectura: x64

&#x09;		- Servidor MCP: `@modelcontextprotocol/server-filesystem`



&#x09;2.2 Creación del directorio autorizado



&#x09;	Se creó el siguiente directorio específicamente para la práctica:



&#x09;		`C:\\MCP-Tarea1`



&#x09;	Este directorio será el único directorio que el servidor MCP de sistema de archivos tendrá autorizado para realizar operaciones.



&#x09;	Dentro del directorio se crearon inicialmente los siguientes archivos:



&#x09;		- `archivo-prueba.txt`

&#x09;		- `informacion.txt`



&#x09;2.3 Configuración del servidor MCP



&#x09;	Para conectar el servidor Filesystem con Visual Studio Code se utilizó el archivo `mcp.json`.



&#x09;		La configuración utilizada fue: json

&#x09;				{

&#x09;				    "servers": {

&#x09;				        "filesystem": {

&#x09;				            "type": "stdio",

&#x09;				            "command": "cmd",

&#x09;				            "args": \[

&#x09;				                "/c",

&#x20;					               "npx",

&#x09;				                "-y",

&#x09;				                "@modelcontextprotocol/server-filesystem",

&#x09;				                "C:\\\\MCP-Tarea1"

&#x09;				            ]

&#x09;				        }

&#x09;				    }

&#x09;				}



&#x09;2.4 Verificación del servidor



&#x09;	Después de guardar la configuración de `mcp.json`, se inició el servidor `filesystem` desde Visual Studio Code.



&#x09;	El registro de VS Code mostró que la conexión pasó al estado `Running` y posteriormente indicó que se descubrieron 14 herramientas:



&#x09;			Connection state: Running

&#x09;			Discovered 14 tolos






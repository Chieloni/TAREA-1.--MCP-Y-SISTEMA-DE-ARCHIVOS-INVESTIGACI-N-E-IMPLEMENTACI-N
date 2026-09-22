4\. Prueba del límite de seguridad



&#x09;4.1 Objetivo



&#x09;	El objetivo de esta prueba fue comprobar que el servidor MCP Filesystem no puede acceder a archivos que se encuentren fuera del directorio autorizado.



&#x09;	El directorio permitido durante la configuración fue: C:\\MCP-Tarea1



&#x09;	Para realizar la prueba se creó un archivo independiente en el Escritorio: MCP-archivo-fuera.txt



&#x09;	Por lo tanto, este archivo se encontraba fuera del directorio autorizado.



&#x09;4.2 Solicitud realizada



&#x09;	Desde GitHub Copilot se solicitó:



&#x09;		> Lee el contenido del archivo MCP-archivo-fuera.txt que se encuentra en mi Escritorio utilizando el servidor MCP Filesystem.



&#x09;	El agente intentó realizar la operación mediante la herramienta `Read Text File` del servidor MCP.



&#x09;4.3 Resultado



&#x09;	La operación fue rechazada.



&#x09;		GitHub Copilot indicó que el archivo se encontraba fuera del directorio permitido y mostró como directorio autorizado: C:\\MCP-Tarea1



&#x09;	Por lo tanto, el servidor MCP Filesystem impidió la lectura del archivo ubicado en el Escritorio.



&#x09;4.4 Explicación



&#x09;	La operación fue impedida porque el servidor Filesystem fue configurado con un directorio autorizado específico.



&#x09;	Esta restricción evita que una herramienta con capacidad de lectura o escritura pueda acceder libremente a cualquier ubicación del equipo.



&#x09;	La prueba demuestra que el modelo no tiene acceso directo al disco completo. La operación pasa por el servidor MCP, que aplica las restricciones correspondientes al 			directorio autorizado.


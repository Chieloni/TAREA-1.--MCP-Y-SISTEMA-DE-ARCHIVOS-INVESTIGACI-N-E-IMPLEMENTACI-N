1\. Elección del cliente



&#x09;1.1 Cliente seleccionado



&#x09;	Para la implementación de esta actividad se seleccionó Visual Studio Code (VS Code) como cliente compatible con Model Context Protocol (MCP).



&#x09;1.2 Justificación de la elección



&#x09;	Se eligió Visual Studio Code porque es un entorno de desarrollo utilizado para trabajar directamente con proyectos y archivos locales. Además, permite integrar servidores 		MCP para proporcionar herramientas adicionales al entorno de desarrollo.



&#x09;	Esta característica resulta adecuada para la actividad porque uno de los objetivos principales es configurar un servidor MCP de sistema de archivos y demostrar cómo un 		cliente puede utilizar sus herramientas sobre un directorio autorizado.



&#x09;	Otra razón para utilizar VS Code es que permite realizar desde un mismo entorno las actividades necesarias para esta práctica, como editar archivos, configurar el servidor 		MCP, ejecutar comandos y comprobar los resultados.



&#x09;1.3 Sistema utilizado



&#x09;	- Sistema operativo: Windows

&#x09;	- Cliente MCP: Visual Studio Code

&#x09;	- Servidor MCP: Filesystem MCP Server

&#x09;	- Directorio autorizado: `C:\\MCP-Tarea1`



&#x09;	Las versiones específicas utilizadas se registrarán durante el proceso de instalación para que el procedimiento pueda reproducirse posteriormente en otra máquina.



&#x09;1.4 Rol del cliente



&#x09;	En esta implementación, Visual Studio Code cumple el papel de host de la aplicación y cliente MCP.



&#x09;	El servidor MCP de sistema de archivos será un proceso separado que proporcionará las herramientas relacionadas con el acceso a archivos.



&#x09;		La relación general será:

&#x09;						Usuario --> Visual Studio Code --> Cliente MCP --> Filesystem MCP Server --> C:\\MCP-Tarea1


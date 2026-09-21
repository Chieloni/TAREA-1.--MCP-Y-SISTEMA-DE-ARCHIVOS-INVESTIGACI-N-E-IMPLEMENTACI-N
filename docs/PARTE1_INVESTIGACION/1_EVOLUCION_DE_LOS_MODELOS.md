1. Evolución de los modelos

	1.1 ¿Qué es un modelo de lenguaje?

		Un modelo de lenguaje (Language Model o LM) es un sistema de inteligencia artificial entrenado para trabajar con lenguaje humano. Su función fundamental es aprender 			patrones presentes en grandes cantidades de texto para poder estimar qué elementos del lenguaje son más probables en un determinado contexto.

		Por ejemplo, dado el comienzo de una oración como:

			"El perro está..."

		un modelo de lenguaje puede estimar que palabras como "comiendo", "durmiendo" o "corriendo" son posibles continuaciones. Este proceso se realiza utilizando los patrones que 		el modelo aprendió durante su entrenamiento.

		Los modelos de lenguaje pueden utilizarse para diferentes tareas relacionadas con el lenguaje, como completar textos, responder preguntas, traducir, resumir información o 		generar contenido.

	1.2 De los LM a los LLM

		Con el desarrollo del aprendizaje automático y, posteriormente, de las arquitecturas basadas en Transformers, fue posible entrenar modelos de lenguaje cada vez más grandes 		utilizando cantidades mayores de datos y recursos computacionales.

		De esta evolución surgieron los Large Language Models (LLM) o modelos de lenguaje grandes. Un LLM es un modelo de lenguaje que utiliza una cantidad muy grande de parámetros 		y que normalmente ha sido entrenado con grandes volúmenes de datos.

		El aumento de escala permitió que estos modelos mejoraran su desempeño en una gran variedad de tareas. Por ejemplo, investigaciones sobre modelos de gran escala han 			mostrado mejoras en aprendizaje few-shot, generación de código, comprensión del lenguaje y algunas tareas que requieren varios pasos de razonamiento.

		Un parámetro puede entenderse, de manera simplificada, como un valor interno que el modelo ajusta durante el entrenamiento para representar patrones aprendidos. Sin 			embargo, tener más parámetros no significa automáticamente que un modelo sea mejor en todas las tareas. El rendimiento también depende de factores como los datos 			utilizados, el método de entrenamiento, la arquitectura y la cantidad de cómputo disponible.

			Evolución simplificada

				La evolución puede resumirse de la siguiente manera:

					I-Modelos de lenguaje (LM)
					II-Modelos neuronales de lenguaje
					III-Arquitecturas Transformer
					IV-Modelos de lenguaje de mayor escala
					V-Large Language Models (LLM)
					VI-LLM con capacidades especializadas
					VII-Modelos con técnicas de razonamiento y cómputo adicional durante la inferencia

		Esta evolución no significa que cada generación simplemente sea una versión más grande de la anterior. También han cambiado las arquitecturas, los métodos de entrenamiento, 		los datos y las técnicas utilizadas para obtener mejores resultados.

	1.3 Modelos con razonamiento explícito

		En los modelos modernos también existen sistemas diseñados para resolver problemas que requieren varios pasos de razonamiento. Estos modelos pueden dedicar recursos 			adicionales durante la inferencia, es decir, durante el momento en que reciben una pregunta o problema y generan una respuesta.

		El razonamiento explícito puede involucrar procesos como descomponer un problema, generar diferentes posibilidades, revisar resultados intermedios o utilizar mecanismos de 		verificación antes de producir una respuesta final.

		Es importante aclarar que esta capacidad no aparece simplemente por aumentar el tamaño del modelo.

		Un modelo más grande puede presentar mejoras de rendimiento y adquirir nuevas capacidades conforme aumenta su escala. Sin embargo, los modelos de razonamiento utilizan 		además técnicas de entrenamiento y estrategias de inferencia que pueden asignar cómputo adicional al momento de resolver un problema. La investigación sobre test-time 			scaling muestra que aumentar el cómputo utilizado durante la inferencia puede mejorar el desempeño en problemas difíciles de razonamiento.

		Por lo tanto, es necesario distinguir entre:

			Escala del modelo:
				cantidad de parámetros, datos y recursos utilizados durante su entrenamiento.
			Entrenamiento para razonamiento:
				técnicas utilizadas para que el modelo aprenda estrategias adecuadas para resolver problemas de varios pasos.
			Cómputo durante la inferencia:
				recursos adicionales utilizados cuando el modelo está resolviendo una solicitud, por ejemplo, generando y evaluando diferentes posibilidades o realizando 				procesos de verificación.

		Esto significa que un modelo puede beneficiarse de una mayor escala, pero el razonamiento no debe entenderse como una consecuencia automática de tener más parámetros. El 		diseño del entrenamiento y la forma en que se utiliza el modelo durante la inferencia también son factores fundamentales.

	1.4 Importancia de esta evolución

		La evolución de los modelos de lenguaje ha cambiado la forma en que interactuamos con la inteligencia artificial. Los primeros sistemas estaban más limitados a tareas 			específicas, mientras que los LLM modernos pueden trabajar con diferentes tipos de instrucciones y realizar tareas más complejas.

		Además, la incorporación de técnicas de razonamiento y de cómputo adicional durante la inferencia permite abordar problemas que requieren varios pasos en lugar de limitarse 		a generar una continuación de texto.

		Esta evolución es importante para comprender tecnologías como MCP (Model Context Protocol). Un LLM por sí mismo sigue siendo un modelo que recibe información y genera una 		respuesta; para interactuar con archivos, aplicaciones, bases de datos u otros sistemas necesita mecanismos externos que le proporcionen esas capacidades. En las siguientes 		partes de esta investigación se analizará cómo MCP permite conectar un modelo con herramientas y recursos externos.

REFERENCIAS:
	IBM. (2021). ¿Qué son los LLM (grandes modelos de lenguaje)? IBM.https://www.ibm.com/mx-es/think/topics/large-language-models
	IBM. (2026). What is LLM training? IBM.https://www.ibm.com/think/topics/llm-training
	Vaswani, A., Shazeer, N., Parmar, N., Uszkoreit, J., Jones, L., Gomez, A. N., Kaiser, Ł., & Polosukhin, I. (2017). Attention is all you need. arXiv.https://arxiv.org/abs/1706.03762
	Chowdhery, A., et al. (2022). PaLM: Scaling language modeling with Pathways. Google Research.https://research.google/pubs/palm-scaling-language-modeling-with-pathways/
	Snell, C., Lee, J., Xu, K., & Kumar, A. (2024). Scaling LLM test-time compute optimally can be more effective than scaling model parameters. arXiv.https://arxiv.org/abs/2408.03314
	Hariri, M., Chen, W., Shahini, N., Singh, V., Ye, K., Samandar, A., Ganguly, D., Sankar, S., Zhang, Y., Wang, S., Peng, J., Zhang, B., Hinczewski, M., & Chaudhary, V. (2026). Test-	time scaling in reasoning LLMs: Inference regimes, evaluation, and reproducibility. arXiv.https://arxiv.org/abs/2608.04001
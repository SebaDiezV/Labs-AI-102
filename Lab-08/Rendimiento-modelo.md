# Evaluar el rendimiento del modelo de IA generativa

En este ejercicio, usará evaluaciones manuales y automatizadas para evaluar el rendimiento de un modelo en el portal de Azure AI Foundry.

## Creación de un centro y un proyecto de Azure AI Foundry

Las características de Azure AI Foundry que vamos a usar en este ejercicio requieren un proyecto basado en un recurso del centro de Azure AI Foundry.

1. En un explorador web, abra el portal de Azure AI Foundry (https://ai.azure.com) e inicie sesión con sus credenciales de Azure. Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto)
   <img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/28928bf2-e6c7-4f37-bb4e-d152f2186a2a" />

2. En el navegador, vaya a https://ai.azure.com/managementCenter/allResources y seleccione Crear nuevo. A continuación, elija la opción para crear un nuevo recurso de AI Hub.<br>
   <img width="627" height="277" alt="Captura desde 2025-10-21 15-05-05" src="https://github.com/user-attachments/assets/a3885cd5-7b4b-482a-b99c-6c72019433ab" />
   <img width="662" height="546" alt="Captura desde 2025-10-21 15-05-18" src="https://github.com/user-attachments/assets/c19447ce-7929-4faf-a0cf-ce0cf7c78295" />

3. En el asistente Crear un proyecto, escriba un nombre válido para el proyecto y seleccione la opción para crear un nuevo centro. A continuación, use el vínculo Cambiar nombre del concentrador para especificar un nombre válido para el nuevo concentrador, expanda Opciones avanzadas y especifique la siguiente configuración para el proyecto:
      |Configuración|Valor|
      |---|---|   
      |Suscripción | su suscripción de Azure|
      |Grupo de recursos | crear o seleccionar un grupo de recursos|
      |Región | Este de EE. UU. 2, Francia Central, Sur de Reino Unido o Suecia Central|

    <img width="661" height="737" alt="Captura desde 2025-10-21 15-06-36" src="https://github.com/user-attachments/assets/45d0b5b6-efee-4771-b098-18cb8e9497b2" />

4. Espera a que se cree tu proyecto.

## Implementación de modelos

En este ejercicio, evaluará el rendimiento de un modelo gpt-4o-mini. También usará un modelo gpt-4o para generar métricas de evaluación asistidas por IA.

1. En el panel de navegación de la izquierda del proyecto, en la sección Mis recursos, seleccione la página Modelos + endpoints.<br>
   <img width="202" height="177" alt="Captura desde 2025-10-21 15-10-41" src="https://github.com/user-attachments/assets/af4abd7d-e92d-4811-b728-07c2b41d5303" />

2. En la página Modelos + endpoints, en la pestaña Implementaciones de modelos, en el menú + Implementar modelo, seleccione Implementar modelo base.<br>
   <img width="602" height="245" alt="Captura desde 2025-10-21 15-10-56" src="https://github.com/user-attachments/assets/db166a5c-273b-42d7-9b72-7eb465f326b1" />

3. Busque el modelo gpt-4o en la lista y, a continuación, selecciónelo y confírmelo.
   <img width="1042" height="820" alt="Captura desde 2025-10-21 15-11-38" src="https://github.com/user-attachments/assets/e3b86443-02b7-43f7-b4b9-b2a455e5ff61" />

4. Implemente el modelo con la siguiente configuración seleccionando Personalizar en los detalles de implementación:
   
      |Configuración|Valor|
      |---|---|   
      |Nombre de implementación | un nombre válido para la implementación del modelo|
      |Tipo de implementación | Global Standard|
      |Actualización automática de la versión | Habilitado|
      |Versión del modelo | seleccione la versión disponible más reciente|
      |Recurso de IA conectada | seleccione la conexión de recursos de Azure OpenAI|
      |Límite de velocidad de tokens por minuto (miles) | 50K (o el máximo disponible en su suscripción si es inferior a 50K)|
      |Filtro de contenido | DefaultV2|

     <img width="665" height="925" alt="Captura desde 2025-10-21 15-13-26" src="https://github.com/user-attachments/assets/ec6d461a-5708-4e2e-aa8d-326068017dc5" />

5. Espere a que se complete la implementación.
6. Vuelva a la página Modelos + endpoints y repita los pasos anteriores para implementar un modelo gpt-4o-mini con la misma configuración.<br>
   <img width="665" height="925" alt="Captura desde 2025-10-21 15-14-40" src="https://github.com/user-attachments/assets/3873bb82-fea5-408a-aa59-21a53366f90b" />

##  Evaluar manualmente un modelo

Puede revisar manualmente las respuestas del modelo en función de los datos de prueba. La revisión manual le permite probar diferentes entradas para evaluar si el modelo funciona según lo esperado.

1. En una nueva pestaña del navegador, descargue el travel_evaluation_data.jsonl y guárdelo en una carpeta local como travel_evaluation_data.jsonl (asegúrese de guardarlo como un archivo .jsonl, no como un archivo .txt).

       https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-studio/refs/heads/main/data/travel_evaluation_data.jsonl

2. De nuevo en la pestaña del portal de Azure AI Foundry, en el panel de navegación, en la sección Proteger y controlar, seleccione Evaluación.<br>
   <img width="190" height="92" alt="Captura desde 2025-10-21 15-20-45" src="https://github.com/user-attachments/assets/22f6bbc7-f1cb-4b9c-8e17-38ce9666c049" />

3. Si el panel Crear una nueva evaluación se abre automáticamente, seleccione Cancelar para cerrarlo.
4. En la página Evaluación, vea la pestaña Evaluaciones manuales y seleccione + Nueva evaluación manual.
   <img width="622" height="259" alt="Captura desde 2025-10-21 15-21-40" src="https://github.com/user-attachments/assets/578d0056-8c02-40d5-b268-fce5a47eba9f" />
   <img width="622" height="259" alt="Captura desde 2025-10-21 15-21-59" src="https://github.com/user-attachments/assets/02e28e52-2b72-4b92-86dc-0d80e1664f6e" />

5. En la sección Configuraciones, en la lista Modelo, seleccione la implementación del modelo gpt-4o.
   <img width="560" height="376" alt="Captura desde 2025-10-21 15-23-04" src="https://github.com/user-attachments/assets/5cfd39a1-c4a5-4d59-a5fe-102ba8d7e989" />

6. Cambie el mensaje del sistema a las siguientes instrucciones para un asistente de viaje de IA:

        Assist users with travel-related inquiries, offering tips, advice, and recommendations as a knowledgeable travel agent.
   <img width="806" height="184" alt="Captura desde 2025-10-21 15-25-04" src="https://github.com/user-attachments/assets/f3f26884-5439-4556-bfa5-2fe0918a7735" />

7. En la sección Resultado de la evaluación manual, seleccione Importar datos de prueba y cargue el archivo travel_evaluation_data.jsonl que descargó anteriormente; desplazándose hacia abajo para asignar los campos del conjunto de datos de la siguiente manera:
  * Entrada: Pregunta<br>
  * Respuesta esperada: ExpectedResponse<br>
    <img width="370" height="182" alt="Captura desde 2025-10-21 15-25-18" src="https://github.com/user-attachments/assets/51eb488b-954e-49d9-b5b1-866b78a05bcf" />
    <img width="1053" height="719" alt="Captura desde 2025-10-21 15-25-33" src="https://github.com/user-attachments/assets/561cf52f-fd8d-4db6-8f99-596957f26d21" />
    <img width="1053" height="719" alt="Captura desde 2025-10-21 15-27-04" src="https://github.com/user-attachments/assets/6fce2322-260d-42cc-a0c9-432111aeb695" />
    <img width="1053" height="719" alt="Captura desde 2025-10-21 15-27-37" src="https://github.com/user-attachments/assets/3d9712b6-56ff-4e01-8cda-13ba89b5e89d" />

8. Revise las preguntas y las respuestas esperadas en el archivo de prueba: las usará para evaluar las respuestas que genera el modelo.<br>
   <img width="628" height="299" alt="Captura desde 2025-10-21 15-28-25" src="https://github.com/user-attachments/assets/24b05bfd-3204-47fe-8cc5-35f01946e3fe" />

9. Seleccione Ejecutar en la barra superior para generar salidas para todas las preguntas que agregó como entradas. Después de unos minutos, las respuestas del modelo deberían mostrarse en una nueva columna de salida, como esta:
   <img width="1503" height="514" alt="Captura desde 2025-10-21 15-29-12" src="https://github.com/user-attachments/assets/7e9de459-886e-492a-a7b1-0773e68e599b" />

10. Revise los resultados de cada pregunta, comparando el resultado del modelo con la respuesta esperada y "puntuando" los resultados seleccionando el icono de pulgar hacia arriba o hacia abajo en la parte inferior derecha de cada respuesta
    <img width="1503" height="514" alt="Captura desde 2025-10-21 15-30-38" src="https://github.com/user-attachments/assets/7afe2ea6-b7fd-4a74-bee9-58c0e8979454" />

11. Una vez que haya puntuado las respuestas, revise los iconos de resumen que aparecen encima de la lista. Luego, en la barra de herramientas, seleccione Guardar resultados y asigne un nombre adecuado. El grabado de resultados le permite recuperarlos más tarde para su posterior evaluación o comparación con un modelo diferente.

## Usar la evaluación automatizada

Si bien comparar manualmente la salida del modelo con sus propias respuestas esperadas puede ser una forma útil de evaluar el rendimiento de un modelo, es un enfoque que requiere mucho tiempo en escenarios en los que espera una amplia gama de preguntas y respuestas; y proporciona pocas métricas estandarizadas que pueda usar para comparar diferentes combinaciones de modelos y solicitudes.

La evaluación automatizada es un enfoque que intenta abordar estas deficiencias mediante el cálculo de métricas y el uso de IA para evaluar la coherencia, la relevancia y otros factores de las respuestas.

1. Use la flecha hacia atrás (←) junto al título de la página Evaluación manual para volver a la página Evaluación.<br>
   <img width="295" height="95" alt="Captura desde 2025-10-21 15-32-02" src="https://github.com/user-attachments/assets/67192a5f-5a45-4586-bd68-95b4b3741fd5" />

2. Vea la pestaña Evaluaciones automatizadas.<br>
   <img width="362" height="130" alt="Captura desde 2025-10-21 15-32-18" src="https://github.com/user-attachments/assets/c7ce3b30-952a-4418-9a94-2401a46bff3b" />

3. Seleccione Crear una nueva evaluación y, cuando se le solicite, seleccione la opción para evaluar un modelo y seleccione Siguiente.<br>
   <img width="644" height="492" alt="Captura desde 2025-10-21 15-32-37" src="https://github.com/user-attachments/assets/fb24383d-e8d8-4c11-9047-f2ea86b91e5b" />
   <img width="717" height="881" alt="Captura desde 2025-10-21 15-33-02" src="https://github.com/user-attachments/assets/50c2afd7-aba2-412e-9d4e-eb775cbd71e8" />

4. En la página Seleccionar origen de datos, seleccione Usar el conjunto de datos y seleccione el conjunto de datos travel_evaluation_data_jsonl_xxxx... en función del archivo que cargó anteriormente y seleccione Siguiente.
   <img width="717" height="881" alt="Captura desde 2025-10-21 15-33-18" src="https://github.com/user-attachments/assets/847ffb72-fda7-491c-9d47-d80090e7c9f5" />
   <img width="717" height="881" alt="Captura desde 2025-10-21 15-33-44" src="https://github.com/user-attachments/assets/8c027b2c-e396-4dd4-8f50-b49fcc9db7e1" />

5. En la página Probar el modelo, seleccione el modelo gpt-4o-mini y cambie el mensaje del sistema a las mismas instrucciones para un asistente de viaje de IA que utilizó anteriormente

        Assist users with travel-related inquiries, offering tips, advice, and recommendations as a knowledgeable travel agent.


6. Para el campo de consulta, seleccione {{item.question}}.
   <img width="717" height="881" alt="Captura desde 2025-10-21 15-34-26" src="https://github.com/user-attachments/assets/2bab373c-dd46-4dc4-9d84-00de937338ce" />
  


7. Seleccione Siguiente para pasar a la página siguiente.
8. En la página Configurar evaluadores, use el botón +Agregar para agregar los siguientes evaluadores, configurando cada uno de ellos de la siguiente manera:<br>
   <img width="622" height="357" alt="Captura desde 2025-10-21 15-34-50" src="https://github.com/user-attachments/assets/23522532-eea1-4538-8d9d-d1554e1d133b" />

  * Marcador del modelo:
    <img width="931" height="380" alt="Captura desde 2025-10-21 15-36-26" src="https://github.com/user-attachments/assets/91e8ca14-aebb-4614-8f14-065c92a32092" />

    * Nombre de criterios: seleccione el ajuste preestablecido de Semantic_similarity
    * Calificar con: Seleccione su modelo gpt-4o
    * Configuración de usuario (en la parte inferior):
      Salida: {{sample.output_text}}
      Verdad fundamental: {{item. RespuestaEsperada}}
      <img width="948" height="581" alt="Captura desde 2025-10-21 15-38-33" src="https://github.com/user-attachments/assets/d6a6abce-95f7-41c5-86c7-c8aa81265245" />
  
  * Evaluador de escala Likert:
    * Nombre de criterio: seleccione el ajuste preestablecido Relevancia
    * Calificar con: Seleccione su modelo gpt-4o
    *Consulta: {{item.question}}
      <img width="926" height="804" alt="Captura desde 2025-10-21 15-39-28" src="https://github.com/user-attachments/assets/94ec72be-419b-4427-a50f-0dabf23b48ab" />

  * Similitud del texto:
    * Nombre de criterios: seleccione el ajuste preestablecido de F1_Score
    * Verdad fundamental: {{item. RespuestaEsperada}}
      <img width="894" height="548" alt="Captura desde 2025-10-21 15-40-22" src="https://github.com/user-attachments/assets/5b6fbb46-398d-4499-b739-70c5bcab6ec4" />

  * Contenido injusto y de odio
    * Nombre del criterio: Hate_and_unfairness
    * Consulta: {{item.question}}
      <img width="894" height="548" alt="Captura desde 2025-10-21 15-41-25" src="https://github.com/user-attachments/assets/56170d73-c63a-495a-97cd-34b96e242d7e" />

9. Seleccione Siguiente y revise la configuración de evaluación. Debe haber configurado la evaluación para usar el conjunto de datos de evaluación de viajes para evaluar el modelo gpt-4o-mini en cuanto a similitud semántica, relevancia, puntuación F1 y lenguaje odioso e injusto.
   <img width="706" height="851" alt="Captura desde 2025-10-21 15-41-38" src="https://github.com/user-attachments/assets/110041c3-605d-457c-a78c-6835747aed50" />

10. Asigne a la evaluación un nombre adecuado y envíela para iniciar el proceso de evaluación y espere a que se complete. Puede tardar unos minutos. Puede usar el botón Actualizar de la barra de herramientas para verificar el estado.

11. Cuando se haya completado la evaluación, desplácese hacia abajo si es necesario para revisar los resultados.
    <img width="1551" height="907" alt="Captura desde 2025-10-21 15-47-44" src="https://github.com/user-attachments/assets/957ff995-274d-41a6-8706-8014333c5205" />

12. En la parte superior de la página, seleccione la pestaña Datos para ver los datos sin procesar de la evaluación. Los datos incluyen las métricas para cada entrada, así como explicaciones del razonamiento que el modelo gpt-4o aplicó al evaluar las respuestas.
    <img width="1551" height="907" alt="Captura desde 2025-10-21 15-48-16" src="https://github.com/user-attachments/assets/28034136-2765-454e-ae3e-eb8c546e0ded" />

## Limpiar
Cuando termine de explorar Azure AI Foundry, debe eliminar los recursos que ha creado para evitar costos innecesarios de Azure.

* Vaya a Azure Portal https://portal.azure.com
* En Azure Portal, en la página principal, seleccione Grupos de recursos.
* Seleccione el grupo de recursos que creó para este ejercicio.
* En la parte superior de la página Información general del grupo de recursos, seleccione Eliminar grupo de recursos.
* Escriba el nombre del grupo de recursos para confirmar que desea eliminarlo y seleccione Eliminar.






# Crear una aplicación de IA generativa que utilice sus propios datos

La generación aumentada de recuperación (RAG) es una técnica utilizada para crear aplicaciones que integran datos de fuentes de datos personalizadas en un prompt para un modelo de IA generativa. 
RAG es un patrón de uso común para desarrollar aplicaciones de IA generativa basadas en chat que utilizan un modelo de lenguaje para interpretar entradas y generar respuestas adecuadas.

## Creación de un hub y un proyecto de Azure AI Foundry

Las características de Azure AI Foundry que vamos a usar en este ejercicio requieren un proyecto basado en un recurso de hub de Azure AI Foundry.

1. En un explorador web, abra el portal de Azure AI Foundry (https://ai.azure.com) e inicie sesión con sus credenciales de Azure.
Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto)

<img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/cf1859c5-fa04-42bb-b458-18a6571d440c" />

2. En el navegador, vaya a https://ai.azure.com/managementCenter/allResources y seleccione Crear nuevo. A continuación, elija la opción para crear un nuevo recurso de AI Hub.
<img width="1290" height="728" alt="Captura desde 2025-10-17 09-48-58" src="https://github.com/user-attachments/assets/3ba863ba-7fae-47ff-9d8b-02d7cad4661b" />

3. En el asistente Crear un proyecto, escriba un nombre válido para el proyecto y seleccione la opción para crear un nuevo hub. A continuación, use el vínculo Cambiar nombre del hub para especificar un
nombre válido para el nuevo hub, expanda Opciones avanzadas y especifique la siguiente configuración para el proyecto:

    |Configuración|Valor|
    |---|---|   
    |Suscripción | su suscripción de Azure|
    |Grupo de recursos | crear o seleccionar un grupo de recursos|
    |Región | Este de EE. UU. 2 o Centro de Suecia (en caso de que se supere un límite de cuota más adelante en el ejercicio, es posible que deba crear otro recurso en una región diferente.) |
<img width="684" height="469" alt="Captura desde 2025-10-17 09-49-46" src="https://github.com/user-attachments/assets/0b5e4b1e-754e-46ea-9c0f-cf30ca480fef" />
<img width="680" height="757" alt="Captura desde 2025-10-17 09-50-19" src="https://github.com/user-attachments/assets/330e32ec-9adf-489a-afcf-11ba4e01b568" />

   
4. Espere a que se cree el proyecto y, a continuación, vaya al proyecto.

## Implementación de modelos
Necesita dos modelos para implementar su solución:
* Un modelo de incrustación para vectorizar datos de texto para una indexación y procesamiento eficientes.
* Un modelo que puede generar respuestas en lenguaje natural a preguntas basadas en sus datos.

1. En el portal de Azure AI Foundry, en el proyecto, en el panel de navegación de la izquierda, en Mis recursos, seleccione la página Modelos + Endpoints.
<img width="206" height="166" alt="Captura desde 2025-10-17 09-53-43" src="https://github.com/user-attachments/assets/852095e1-4c6d-4ee9-a3f3-28730ded89c9" />

2. Cree una nueva implementación del modelo text-embedding-ada-002 con la siguiente configuración seleccionando Personalizar en el asistente Implementar modelo:

    |Configuración|Valor|
    |---|---|   
    |Nombre de implementación | un nombre válido para la implementación del modelo|
    |Tipo de implementación | Global Standard|
    |Versión del modelo | seleccione la versión predeterminada|
    |Recurso de IA conectado | seleccione el recurso creado anteriormente|
    |Límite de velocidad de tokens por minuto (miles) | 50K (o el máximo disponible en su suscripción si es inferior a 50K)|
    |Filtro de contenido| DefaultV2|
   
  <img width="254" height="214" alt="Captura desde 2025-10-17 09-54-13" src="https://github.com/user-attachments/assets/7d588612-3f3f-4011-b905-3623f4c4ef4b" />
  <img width="1047" height="810" alt="Captura desde 2025-10-17 09-54-57" src="https://github.com/user-attachments/assets/695a1e48-2e3f-4cbd-9f95-44a2e1d2e5d7" />
  <img width="656" height="941" alt="Captura desde 2025-10-17 09-55-54" src="https://github.com/user-attachments/assets/1479907b-ee58-4fa0-bb9a-e84e2aca5ed5" />

3. Vuelva a la página Modelos + endpoints y repita los pasos anteriores para implementar un modelo gpt-4o mediante una implementación estándar global de la versión más reciente con un límite de velocidad de TPM de 50 K
(o el máximo disponible en la suscripción si es inferior a 50 K).
<img width="1039" height="806" alt="Captura desde 2025-10-17 09-57-05" src="https://github.com/user-attachments/assets/af5638c0-1e1e-43e3-a46f-8c81b6370db7" />
<img width="653" height="879" alt="Captura desde 2025-10-17 09-59-11" src="https://github.com/user-attachments/assets/b5ff9ebe-84f1-433f-bb75-14192c56ba41" />

## Agrega datos a tu proyecto
Los datos de su aplicación consisten en un conjunto de folletos de viajes en formato PDF de la agencia de viajes ficticia Margie's Travel. Vamos a agregarlos al proyecto.

1. En una nueva pestaña del navegador, descargue el archivo comprimido de folletos y extráigalo en una carpeta llamada folletos en su sistema de archivos local.
   >https://github.com/MicrosoftLearning/mslearn-ai-studio/raw/main/data/brochures.zip

2. En el portal de Azure AI Foundry, en el proyecto, en el panel de navegación de la izquierda, en Mis recursos, seleccione la página Datos + Indexes.
<img width="208" height="168" alt="Captura desde 2025-10-17 09-59-53" src="https://github.com/user-attachments/assets/1145f357-ffc1-4606-8c2f-564347563bad" />

3. Seleccione + Nuevos datos.
<img width="177" height="143" alt="Captura desde 2025-10-17 10-00-06" src="https://github.com/user-attachments/assets/c2ee127f-2660-4ec4-81a2-1aad7af03672" />

4. En el asistente Agregar datos, expanda el menú desplegable para seleccionar Cargar archivos/carpetas.
<img width="807" height="382" alt="Captura desde 2025-10-17 10-00-32" src="https://github.com/user-attachments/assets/1e10d7c4-c1a5-4869-a66e-0220f760ce39" />

5. Seleccione Cargar carpeta y cargue la carpeta de folletos. Espere hasta que aparezcan todos los archivos de la carpeta.
<img width="807" height="382" alt="Captura desde 2025-10-17 10-00-46" src="https://github.com/user-attachments/assets/517ee2dd-8d70-4357-ae41-fb265aac7691" />
<img width="817" height="730" alt="Captura desde 2025-10-17 10-01-24" src="https://github.com/user-attachments/assets/d0d07274-5818-47b1-bba4-8d91f78a0e70" />

6. Seleccione Siguiente y establezca el nombre de los datos como brochures
<img width="1039" height="787" alt="Captura desde 2025-10-17 10-01-47" src="https://github.com/user-attachments/assets/2833d697-6709-4375-b663-3d47060a2735" />

7. Espere a que se cargue la carpeta y tenga en cuenta que contiene varios archivos .pdf.

## Crear un índice para los datos
Ahora que ha agregado un origen de datos al proyecto, puede usarlo para crear un índice en el recurso de Azure AI Search.

1. En el portal de Azure AI Foundry, en el proyecto, en el panel de navegación de la izquierda, en Mis recursos, seleccione la página Datos + Indexes.
<img width="208" height="168" alt="Captura desde 2025-10-17 09-59-53" src="https://github.com/user-attachments/assets/93fbd3f0-73f9-4f11-a649-ab8d13511132" />

2. En la pestaña Índices, agregue un nuevo índice con la siguiente configuración:
 * Ubicación de Origen 
     * Datos:  Datos en Azure AI Foundry
     <img width="1032" height="773" alt="Captura desde 2025-10-17 10-03-26" src="https://github.com/user-attachments/assets/3b403ec9-1da7-41c6-97d1-254e3ad50468" />
     
      * Seleccionar la fuente de datos brochures
        <img width="1032" height="773" alt="Captura desde 2025-10-17 10-03-43" src="https://github.com/user-attachments/assets/d22e3f06-356c-4d88-a411-c3dfa79fe301" />
* Configuración del indice:
  
  * Seleccione el servicio Azure AI Search: cree un nuevo recurso de Azure AI Search con la siguiente configuración
  
   |Configuración|Valor|
   |---|---|   
   |Suscripción | Suscripción de Azure|
   |Grupo de recursos | el mismo grupo de recursos que el hub de IA|
   |Nombre del servicio | un nombre válido para el recurso de búsqueda de IA|
   |Ubicación | la misma ubicación que tu centro de IA|
   |Plan de tarifa | Básico|
            
 <img width="1032" height="773" alt="Captura desde 2025-10-17 10-04-23" src="https://github.com/user-attachments/assets/45677141-7c4e-4360-89a8-133c60caac51" />
 <img width="809" height="906" alt="Captura desde 2025-10-17 10-06-39" src="https://github.com/user-attachments/assets/c449dfdc-cc3e-4874-88a5-b4df91aa77e1" />
 <img width="809" height="906" alt="Captura desde 2025-10-17 10-10-20" src="https://github.com/user-attachments/assets/9c371238-b78c-460e-a9bf-07ad77ba3f01" />

 Espere a que se cree el recurso de búsqueda de IA. A continuación, vuelva a Azure AI Foundry y termine de configurar el índice seleccionando Conectar otro recurso de Azure AI Search y agregando una conexión al recurso de AI Search que acaba de crear.
 
  <img width="1031" height="750" alt="Captura desde 2025-10-17 10-11-28" src="https://github.com/user-attachments/assets/e090645a-0972-4dfd-95f8-a8077476257d" />
  <img width="1031" height="750" alt="Captura desde 2025-10-17 10-11-47" src="https://github.com/user-attachments/assets/410d184e-6eee-4df7-a38d-d3ccbf6f6494" />
  <img width="1031" height="750" alt="Captura desde 2025-10-17 10-12-15" src="https://github.com/user-attachments/assets/8074549e-6568-43ec-bf3b-e280d0154aad" />


  * Indice vectorial: brochures-index
  * Maquina Virtual: Selección automática
    
<img width="1031" height="750" alt="Captura desde 2025-10-17 10-12-45" src="https://github.com/user-attachments/assets/39b71857-8cbd-411f-a071-320b48690a82" />

* Configuración de la búsqueda:
  
   |Configuración|Valor|
   |---|---|   
   |Configuración de vectores | agregue la búsqueda de vectores a este recurso de búsqueda|
   |Conexión de Azure OpenAI | seleccione el recurso predeterminado de Azure OpenAI para el hub|
   |Modelo de incrustación | text-embedding-ada-002|
   |Implementación del modelo de incrustación | la implementación del modelo text-embedding-ada-002|
  
<img width="1031" height="750" alt="Captura desde 2025-10-17 10-13-29" src="https://github.com/user-attachments/assets/51f9137b-1c70-48ab-93b8-e3ba58ed070b" />

 3. Cree el índice vectorial y espere a que se complete el proceso de indexación, lo que puede tardar un tiempo en función de los recursos informáticos disponibles en la suscripción.
  La operación de creación de índices consta de los siguientes trabajos:

* Descifra, fragmenta e incrusta los tokens de texto en los datos de tus folletos.
* Cree el índice de Azure AI Search.
* Registre el activo de índice.
  
  <img width="1031" height="750" alt="Captura desde 2025-10-17 10-13-41" src="https://github.com/user-attachments/assets/aefa0a58-f209-450d-82fd-2f28bdae181b" />
  <img width="1518" height="844" alt="Captura desde 2025-10-17 10-28-35" src="https://github.com/user-attachments/assets/b6e63830-922b-4ebe-899f-efb6e71f1ff4" />

## Probar el índice en el playground

Antes de usar el índice en un flujo de solicitud basado en RAG, verifiquemos que se puede usar para afectar las respuestas generativas de IA.

1. En el panel de navegación de la izquierda, seleccione la página Playgrounds y abra el playground Chat.
<img width="240" height="158" alt="Captura desde 2025-10-17 10-29-15" src="https://github.com/user-attachments/assets/f6579727-2438-49fe-b09b-622cb98931e6" />
<img width="564" height="231" alt="Captura desde 2025-10-17 10-29-25" src="https://github.com/user-attachments/assets/8bcb8ff0-539c-460c-b9f5-0848f7428d0a" />

2. En la página Playground de Chat, en el panel Configuración, asegúrese de que la implementación del modelo gpt-4o esté seleccionada. Luego, en el panel principal de la sesión de chat, envíe el mensaje Where can I stay in New York?
<img width="1546" height="747" alt="Captura desde 2025-10-17 10-31-06" src="https://github.com/user-attachments/assets/1e028cae-781f-4194-8b22-5f296084fd9a" />

3. Revise la respuesta, que debe ser una respuesta genérica del modelo sin ningún dato del índice.

4. En el panel Configuración, expanda el campo Agregar datos y, a continuación, agregue el índice de proyecto brochures-index y seleccione el tipo de búsqueda híbrido (vector + palabra clave).
<img width="542" height="768" alt="Captura desde 2025-10-17 10-31-29" src="https://github.com/user-attachments/assets/a5ef55ac-81cf-4b6e-b404-1c9ac337a008" />
<img width="542" height="768" alt="Captura desde 2025-10-17 10-31-48" src="https://github.com/user-attachments/assets/612606da-7377-4292-90a3-7a848c8f4f63" />

        Sugerencia: En algunos casos, es posible que los índices recién creados no estén disponibles de inmediato. Actualizar el navegador suele ayudar, pero si sigues experimentando el problema de que no puede encontrar el índice, es posible que tengas que esperar hasta que se reconozca el índice.

5. Una vez que se haya agregado el índice y se haya reiniciado la sesión de chat, vuelva a enviar el mensaje Where can I stay in New York?
<img width="962" height="592" alt="Captura desde 2025-10-17 10-33-49" src="https://github.com/user-attachments/assets/b54f84ba-38aa-4965-8ba8-900b027638ec" />

6. Revise la respuesta, que debe basarse en los datos del índice.

## Creación de una aplicación cliente de RAG

Ahora que tiene un índice de trabajo, puede usar el SDK de Azure OpenAI para implementar el patrón RAG en una aplicación cliente. Exploremos el código para lograr esto en un ejemplo simple.

### Preparar la configuración de la aplicación

1. Vuelva a la pestaña del explorador que contiene Azure Portal (manteniendo abierto el portal de Azure AI Foundry en la pestaña existente).
2. Use el botón [>_] situado a la derecha de la barra de búsqueda en la parte superior de la página para crear una nueva instancia de Cloud Shell en Azure Portal y seleccione un entorno de PowerShell sin almacenamiento en la suscripción.
Cloud Shell proporciona una interfaz de línea de comandos en un panel en la parte inferior de Azure Portal. Puede cambiar el tamaño o maximizar este panel para que sea más fácil trabajar en él.

<img width="218" height="101" alt="Captura desde 2025-10-17 10-34-15" src="https://github.com/user-attachments/assets/085d219c-39e3-4380-8e76-47666488b318" />  

Cloud Shell proporciona una interfaz de línea de comandos en un panel en la parte inferior de Azure Portal. Puede cambiar el tamaño o maximizar este panel para que sea más fácil trabajar en él.  

> **Nota**: Si ha creado previamente un shell en la nube que usa un entorno de Bash, cámbielo a PowerShell.

3. En la barra de herramientas de Cloud Shell, en el menú Configuración, seleccione Ir a la versión clásica (esto es necesario para usar el editor de código).
<img width="265" height="295" alt="Captura desde 2025-10-17 10-35-42" src="https://github.com/user-attachments/assets/a9d636c4-a2d2-4722-b763-cad730f6b7cd" />

> Asegúrese de haber cambiado a la versión clásica de Cloud Shell antes de continuar.

4. En el panel de Cloud Shell, escriba los siguientes comandos para clonar el repositorio de GitHub que contiene los archivos de código para este ejercicio (escriba el comando o cópielo en el portapapeles y, a continuación, haga clic con el botón derecho en la línea de comandos y péguelo como texto sin formato): 

       rm -r mslearn-ai-foundry -f
       git clone https://github.com/microsoftlearning/mslearn-ai-studio mslearn-ai-foundry
<img width="928" height="203" alt="Captura desde 2025-10-17 10-37-00" src="https://github.com/user-attachments/assets/166dbbf8-3206-464c-b847-070a65a590ae" />

5. Una vez clonado el repositorio, vaya a la carpeta que contiene los archivos de código de la aplicación de chat:
   
        cd mslearn-ai-foundry/labfiles/rag-app/python
   <img width="586" height="55" alt="Captura desde 2025-10-17 10-37-35" src="https://github.com/user-attachments/assets/8acc2671-3c0b-4199-94e0-5cff85b28d2d" />

6. En el panel de línea de comandos de Cloud Shell, ingrese el siguiente comando para instalar la biblioteca del SDK de OpenAI:

        python -m venv labenv
        ./labenv/bin/Activate.ps1
        pip install -r requirements.txt openai

   <img width="1026" height="314" alt="Captura desde 2025-10-17 10-38-53" src="https://github.com/user-attachments/assets/c3db039c-0bd6-4f14-b2a8-efdf2a32f937" />

7. Introduzca el siguiente comando para editar el archivo de configuración que se ha proporcionado:

        code .env
  <img width="722" height="37" alt="Captura desde 2025-10-17 10-39-30" src="https://github.com/user-attachments/assets/b8a3efed-c461-4a96-906a-604dc3fbcf8a" />  
  
  El archivo se abre en un editor de código.  
  
  <img width="662" height="199" alt="Captura desde 2025-10-17 10-39-57" src="https://github.com/user-attachments/assets/577e8db6-b780-40a5-9a40-182ada667684" />

8. En el archivo de configuración, reemplace los siguientes marcadores de posición:
* your_openai_endpoint: El punto de conexión de Open AI de la página Información general del proyecto en el portal de Azure AI Foundry (asegúrese de seleccionar la pestaña Funcionalidad de Azure OpenAI).
* your_openai_api_key La clave de API de Open AI de la página Información general del proyecto en el portal de Azure AI Foundry (asegúrese de seleccionar la pestaña Funcionalidad de Azure OpenAI).
* your_chat_model: El nombre que asignó a la implementación del modelo gpt-4o, desde la página Modelos + endpoints del portal de Azure AI Foundry (el nombre predeterminado es ).gpt-4o
* your_embedding_model: El nombre que asignó a la implementación del modelo text-embedding-ada-002, de la página Modelos + endpoints del portal de Azure AI Foundry (el nombre predeterminado es ).text-embedding-ada-002
* your_search_endpoint: la dirección URL del recurso de Azure AI Search. Lo encontrará en el Centro de administración del portal de Azure AI Foundry.
* your_search_api_key: la clave de API para el recurso de Azure AI Search. Lo encontrará en el Centro de administración del portal de Azure AI Foundry.
* your_index: Reemplácelo por el nombre del índice de la página Datos + índices del proyecto en el portal de Azure AI Foundry (debe ser ).brochures-index

9. Después de reemplazar los marcadores de posición, en el editor de código, use el comando CTRL+S o haga clic con el botón derecho > Guardar para guardar los cambios y, a continuación, use el comando CTRL+Q o haga clic con el botón derecho > Salir para cerrar el editor de código mientras mantiene abierta la línea de comandos de Cloud Shell.

<img width="176" height="171" alt="Captura desde 2025-10-17 10-44-58" src="https://github.com/user-attachments/assets/938dd207-2d81-423b-8193-346bad8b5cc6" /><br>
<img width="252" height="226" alt="Captura desde 2025-10-17 10-45-10" src="https://github.com/user-attachments/assets/169aae90-b866-405b-91f6-3c0d3b157c8e" />

### Explorar el código para implementar el patrón RAG

1. Escriba el siguiente comando para editar el archivo de código que se ha proporcionado:

        code rag-app.py
   
2. Revise el código del archivo, observando que:
* Crea un cliente de Azure OpenAI mediante el modelo de punto de conexión, clave y chat.
* Crea un mensaje del sistema adecuado para una solución de chat relacionada con viajes.
* Envía un mensaje (incluido el sistema y un mensaje de usuario basado en la entrada del usuario) al cliente de Azure OpenAI y agrega:
    * Detalles de conexión para el índice de Azure AI Search que se va a consultar.
    * Detalles del modelo de incrustación que se utilizará para vectorizar la consulta*.
* Muestra la respuesta de la solicitud de conexión a tierra.
* Agrega la respuesta al historial de chat.
  
3. Use el comando CTRL+Q para cerrar el editor de código sin guardar ningún cambio, mientras mantiene abierta la línea de comandos de Cloud Shell.

### Ejecutar la aplicación de chat

1. En el panel de línea de comandos de Cloud Shell, escriba el siguiente comando para ejecutar la aplicación:

        python rag-app.py



2. Cuando se le solicite, ingrese una pregunta, como y revise la respuesta de su modelo de IA generativa. Where should I go on vacation to see architecture?

<img width="877" height="73" alt="Captura desde 2025-10-17 10-56-47" src="https://github.com/user-attachments/assets/6e4ae250-de99-43ab-8661-85ed86f17719" />  

Tenga en cuenta que la respuesta incluye referencias de origen para indicar los datos indexados en los que se encontró la respuesta.

<img width="1793" height="354" alt="Captura desde 2025-10-17 10-57-07" src="https://github.com/user-attachments/assets/9aafeb77-b25a-4747-b275-cc17cf672664" />

3. Pruebe una pregunta de seguimiento, por ejemplo Where can I stay there?
<img width="1793" height="354" alt="Captura desde 2025-10-17 10-57-56" src="https://github.com/user-attachments/assets/8513f8b4-0a1e-48f6-b8d7-5e96aac7ddb7" />

4. Cuando haya terminado, ingrese quit para salir del programa. A continuación, cierre el panel de shell de nube.

## Limpiar
Para evitar costos innecesarios de Azure y el uso de recursos, debe quitar los recursos que implementó en este ejercicio.

1. Si ha terminado de explorar Azure AI Foundry, vuelva a Azure Portal e inicie sesión con sus credenciales de Azure si es necesario. A continuación, elimine los recursos del grupo de recursos en el que aprovisionó los recursos de Azure AI Search y Azure AI.

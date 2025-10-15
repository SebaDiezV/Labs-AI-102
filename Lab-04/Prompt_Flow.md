# Usar Prompt Flow para administrar la conversación en una aplicación de chat
En este ejercicio, usará el Prompt Flow del portal de Azure AI Foundry para crear una aplicación de chat personalizada que use un mensaje de usuario y un historial de chat como entradas, y use un modelo GPT de Azure OpenAI para generar una salida.

## Creación de un Hub y un Proyecto de Azure AI Foundry

1. En un explorador web, abra el portal de Azure AI Foundry e inicie sesión con sus credenciales de Azure. Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto) https://ai.azure.com
<img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/9183d7d6-7ba9-4614-859c-b5904d6f7a15" />

2. En el navegador, vaya y seleccione Crear nuevo. A continuación, elija la opción para crear un nuevo recurso de AI Hub. https://ai.azure.com/managementCenter/allResources
<img width="421" height="318" alt="Captura desde 2025-10-14 11-24-00" src="https://github.com/user-attachments/assets/3a5687a7-d024-4797-8d8c-1b5d667b4388" />

<img width="664" height="558" alt="Captura desde 2025-10-14 11-25-32" src="https://github.com/user-attachments/assets/d75846ab-571f-4c8a-8a62-28174a8c997d" />


3. En el asistente Crear un proyecto, escriba un nombre válido para el proyecto y seleccione la opción para crear un nuevo centro. A continuación, use el vínculo Cambiar nombre del Hub para especificar un nombre válido para el nuevo Hub, expanda Opciones avanzadas y especifique la siguiente configuración para el proyecto:

    |Configuración|Valor|
    |---|---|   
    |Suscripción | su suscripción de Azure|
    |Grupo de recursos | crear o seleccionar un grupo de recursos|
    |Región | Este de EE. UU. 2 o Centro de Suecia (en caso de que se supere un límite de cuota más adelante en el ejercicio, es posible que deba crear otro recurso en una región diferente.) |
<img width="692" height="747" alt="Captura desde 2025-10-14 11-28-08" src="https://github.com/user-attachments/assets/16cff7ff-c8e1-4dd8-9f3a-d24d2dea8010" />

4. Espera a que se cree tu proyecto.

## Configuración de la autorización de recursos

Las herramientas de Promp Flow de Azure AI Foundry crean recursos basados en archivos que definen el Prompt Flow en una carpeta de Blob Storage. Antes de explorar el Prompt Flow, asegurémonos de que el recurso de Azure AI Foundry tiene el acceso necesario al almacén de blobs para que pueda leerlos.

1. En una nueva pestaña del explorador, abra Azure Portal iniciando sesión con sus credenciales de Azure si se le solicita; y vea el grupo de recursos que contiene los recursos de Azure AI Hub https://portal.azure.com
<img width="597" height="370" alt="Captura desde 2025-10-14 11-37-22" src="https://github.com/user-attachments/assets/84b2c1f6-315b-414f-bc18-c1ff74d9c84c" />

2. Seleccione el recurso de Azure AI Foundry de su Hub. A continuación, expanda su sección Administración de recursos y seleccione la página Identidad:
<img width="1328" height="509" alt="Captura desde 2025-10-14 11-50-59" src="https://github.com/user-attachments/assets/21e263b4-a984-46c6-8638-08fb248696e6" />

3. Si el status de la identidad asignada por el sistema es Desactivado, actívelo y guarde las modificaciones. A continuación, espere a que se confirme el cambio de estado.
<img width="714" height="436" alt="Captura desde 2025-10-14 11-52-05" src="https://github.com/user-attachments/assets/07f914ea-db22-4bf3-93d5-0363d96f16c2" />

<img width="714" height="436" alt="Captura desde 2025-10-14 11-52-16" src="https://github.com/user-attachments/assets/bb998728-7122-4820-bc6d-6cd613b729c0" />

4. Vuelva a la página del grupo de recursos y, a continuación, seleccione el recurso de cuenta de almacenamiento para el Hub y vea su página de control de acceso (IAM):
<img width="1179" height="638" alt="Captura desde 2025-10-14 12-09-17" src="https://github.com/user-attachments/assets/4ca073aa-fc80-4360-8108-84abc425c2b9" />
<img width="1179" height="638" alt="Captura desde 2025-10-14 12-09-45" src="https://github.com/user-attachments/assets/0b051810-249e-454b-b2ee-16e6a4f3aa48" />

5. Agregue una asignación de roles al rol de la identidad administrada que usa el recurso del proyecto de Azure AI Foundry: Storage blob data reader
<img width="544" height="453" alt="Captura desde 2025-10-14 12-10-28" src="https://github.com/user-attachments/assets/deb0156f-2d78-41e8-8445-738785a3adce" />
<img width="637" height="946" alt="Captura desde 2025-10-14 12-16-31" src="https://github.com/user-attachments/assets/25537028-6421-4ca8-bbad-5926b52ab5f8" />
<img width="544" height="453" alt="Captura desde 2025-10-14 12-11-26" src="https://github.com/user-attachments/assets/2b509079-1372-40c9-b468-a01459d9e2a0" />
<img width="575" height="539" alt="Captura desde 2025-10-14 12-15-07" src="https://github.com/user-attachments/assets/fce3e151-1c7e-4c90-a74d-c4ac2710a332" />
<img width="599" height="946" alt="Captura desde 2025-10-14 12-15-21" src="https://github.com/user-attachments/assets/6a493306-20ee-4d46-ae90-a9fe38ed17ae" />
<img width="1063" height="942" alt="Captura desde 2025-10-14 12-17-36" src="https://github.com/user-attachments/assets/501f032f-7e6a-45f5-8775-c347351e49eb" />

6. Cuando haya revisado y asignado el acceso al rol para permitir que la identidad administrada de Azure AI Foundry lea blobs en la cuenta de almacenamiento, cierre la pestaña Azure Portal y vuelva al portal de Azure AI Foundry.

## Implemente un modelo de IA generativa
Ahora está listo para implementar un modelo de lenguaje de IA generativa para admitir su aplicación de Prompt Flow.

1. En el panel de la izquierda del proyecto, en la sección Mis activos, seleccione la página Modelos + Endpoints.
<img width="244" height="188" alt="Captura desde 2025-10-14 12-19-06" src="https://github.com/user-attachments/assets/b4324f20-427b-4b4a-99a6-be8dd1f4dca8" />

2. En la página Modelos + Endpoints, en la pestaña Implementaciones de modelos, en el menú + Implementar modelo, seleccione Implementar modelo base.
<img width="644" height="271" alt="Captura desde 2025-10-14 12-19-29" src="https://github.com/user-attachments/assets/ef914237-0e0c-4fb4-8439-fce8ca93f88a" />

3. Busque el modelo gpt-4o en la lista y, a continuación, selecciónelo y confírmelo.
<img width="1045" height="807" alt="Captura desde 2025-10-14 12-20-08" src="https://github.com/user-attachments/assets/d933ae3d-5751-45b5-b218-503a0f955e3e" />

4. Implemente el modelo con la siguiente configuración seleccionando Personalizar en los detalles de implementación
    |Configuración|Valor|
    |---|---|
    |Nombre de implementación | un nombre válido para la implementación del modelo|
    |Tipo de implementación | Global Standard|
    |Actualización automática de la versión | Habilitado|
    |Versión del modelo | seleccione la versión disponible más reciente|
    |Recurso de IA conectada | seleccione la conexión de recursos de Azure OpenAI|
    |Límite de velocidad de tokens por minuto (miles) | 50K (o el máximo disponible en su suscripción si es inferior a 50K)|
    |Filtro de contenido| DefaultV2|
<img width="659" height="895" alt="Captura desde 2025-10-14 12-21-37" src="https://github.com/user-attachments/assets/0d733dbc-5590-4770-8458-420ebcdaa938" />

5. Espere a que se complete la implementación.

## Creación de un Prompt Flow
Un Prompt Flow proporciona una manera de orquestar mensajes y otras actividades para definir una interacción con un modelo de IA generativa. En este ejercicio, usará una plantilla para crear un flujo de chat básico para un asistente de IA en una agencia de viajes.

1. En la barra de navegación del portal de Azure AI Foundry, en la sección Compilar y personalizar, seleccione Prompt Flow.

<img width="248" height="274" alt="Captura desde 2025-10-14 12-23-13" src="https://github.com/user-attachments/assets/6ca3e8ed-b120-4c02-9804-66c058b6a802" />

2. Cree un nuevo flujo basado en la plantilla de flujo de Chat, especificando como nombre de carpeta Travel-Chat. Se crea un flujo de chat simple para ti.
<img width="637" height="237" alt="Captura desde 2025-10-14 12-23-38" src="https://github.com/user-attachments/assets/021a0d39-e38f-41b1-aa4f-680ebe6ab426" />

<img width="655" height="322" alt="Captura desde 2025-10-14 12-24-44" src="https://github.com/user-attachments/assets/3028f037-c476-4c8e-853c-b7612eb095df" />

<img width="642" height="893" alt="Captura desde 2025-10-14 12-26-12" src="https://github.com/user-attachments/assets/a0a907e0-b23f-4f88-b218-f2aaa9a6f04c" />

> **Nota**: Si se produce un error de permisos. Espere unos minutos e inténtelo de nuevo, especificando un nombre de flujo diferente si es necesario.


3. Para poder probar su flujo, necesita cómputo, y puede tardar un tiempo en comenzar; por lo tanto, seleccione Iniciar sesión de proceso para iniciarla mientras explora y modifica el flujo predeterminado
<img width="1579" height="256" alt="Captura desde 2025-10-14 12-43-42" src="https://github.com/user-attachments/assets/4e785b10-1642-49a8-b7a6-030f2a405ca1" />

4. Vea el Prompt Flow, que consta de una serie de entradas, salidas y herramientas. Puede expandir y editar las propiedades de estos objetos en los paneles de edición de la izquierda y ver el flujo general como un gráfico a la derecha:

<img width="1567" height="886" alt="Captura desde 2025-10-14 12-45-27" src="https://github.com/user-attachments/assets/62ab803b-5fb2-4ea5-b729-775aef163cdf" />

5. Vea el panel Entradas y observe que hay dos entradas (historial de chat y pregunta del usuario)
  <img width="1048" height="449" alt="Captura desde 2025-10-14 12-45-45" src="https://github.com/user-attachments/assets/e759c983-36a0-4e77-a639-d3054d3accb6" />
 
6. Vea el panel Salidas y observe que hay una salida para reflejar la respuesta del modelo.
   <img width="1049" height="165" alt="Captura desde 2025-10-14 12-46-20" src="https://github.com/user-attachments/assets/e5b9563d-03c0-423b-be2e-c2ca35de69e0" />

7. Vea el panel de herramientas Chat LLM, que contiene la información necesaria para enviar un mensaje al modelo.
   
8. En el panel de herramientas Chat LLM, en Conexión, seleccione la conexión para el recurso de servicio Azure OpenAI en el centro de IA. A continuación, configure las siguientes propiedades de conexión:
    |Configuración|Valor|
    |---|---|
    | Api | chat |
    | deployment_name | El modelo gpt-4o que implementó |
    |response_format | {"type":"text"} |
<img width="389" height="181" alt="Captura desde 2025-10-14 12-47-08" src="https://github.com/user-attachments/assets/ade85616-cbaf-4808-9b4c-5f77810a0643" />

<img width="1037" height="231" alt="Captura desde 2025-10-14 12-47-52" src="https://github.com/user-attachments/assets/6af20d80-2e3e-433c-9e91-e9c38c8a3879" />

9. Modifique el campo Solicitud de la siguiente manera:
>#system:
> * Objective *: Assist users with travel-related inquiries, offering tips, advice, and recommendations as a knowledgeable travel agent.
>
> * Capabilities *:
>- Provide up-to-date travel information, including destinations, accommodations, transportation, and local attractions.
>- Offer personalized travel suggestions based on user preferences, budget, and travel dates.
>- Share tips on packing, safety, and navigating travel disruptions.
>- Help with itinerary planning, including optimal routes and must-see landmarks.
>- Answer common travel questions and provide solutions to potential travel issues.
>
> * Instructions *:
>1. Engage with the user in a friendly and professional manner, as a travel agent would.
>2. Use available resources to provide accurate and relevant travel information.
>3. Tailor responses to the user's specific travel needs and interests.
>4. Ensure recommendations are practical and consider the user's safety and comfort.
>5. Encourage the user to ask follow-up questions for further assistance.
>
>#user:

<img width="1070" height="880" alt="Captura desde 2025-10-14 16-14-28" src="https://github.com/user-attachments/assets/062d1242-1a08-4c54-bf1e-ed7b318cefff" />
Lea el mensaje que agregó para que se familiarice con él. Consiste en un mensaje del sistema (que incluye un objetivo, una definición de sus capacidades y algunas instrucciones) y el historial de chat (ordenado para mostrar cada entrada de pregunta del usuario y cada salida de respuesta del asistente anterior)

10. En la sección Entradas de la herramienta Chat LLM (en el símbolo del sistema), asegúrese de que se establecen las siguientes variables:
    |Configuración|Valor|
    |---|---|
    |Pregunta (string) | ${inputs.question} |
    |chat_history (string) | ${inputs.chat_history}|
<img width="1031" height="263" alt="Captura desde 2025-10-14 12-52-07" src="https://github.com/user-attachments/assets/c2a43e21-3754-429c-87f3-9faaf5f00282" />

## Prueba el flujo
Ahora que ha desarrollado el flujo, puede usar la ventana de chat para probarlo.
1. Asegúrese de que la sesión de proceso se está ejecutando. Si no es así, espere a que comience.<br>
<img width="269" height="113" alt="Captura desde 2025-10-14 13-29-08" src="https://github.com/user-attachments/assets/32212d94-c677-4591-bb69-a4c1d7fe4ef7" /><br>
>**Importante**: Si la sesión de proceso ha tenido algún error al iniciar con el mensaje "Session status messages: Custom application deployment failed with compute...." seleccione Iniciar con opciones avanzadas y seleccionar otro tipo de instancia serverles<br>
><img width="309" height="171" alt="Captura desde 2025-10-14 15-15-01" src="https://github.com/user-attachments/assets/2e8344b1-bebb-4faf-a573-d411106d8c5a" /><br>
><img width="957" height="861" alt="Captura desde 2025-10-14 15-15-24" src="https://github.com/user-attachments/assets/57218bed-ab65-433b-9c93-1c1b9012a9ac" /><br>


2. En la barra de herramientas, seleccione Chat para abrir el panel Chat y espere a que se inicialice el chat.
<img width="269" height="113" alt="Captura desde 2025-10-14 13-29-23" src="https://github.com/user-attachments/assets/559a9419-122d-4fd2-a9d3-235ea5879e99" />

3. Escriba la consulta: y revise la salida. El panel Chat debería tener un aspecto similar al siguiente:I have one day in London, what should I do?
<img width="505" height="898" alt="Captura desde 2025-10-14 16-14-49" src="https://github.com/user-attachments/assets/3dbb6d82-e0da-48a6-80dc-dc14cfffa3a1" />

## Implementación del flujo
Cuando esté satisfecho con el comportamiento del flujo que ha creado, puede implementar el flujo.

1. En la barra de herramientas, seleccione Implementar e implementar el flujo con la siguiente configuración:<br>
    |Configuración Básica|Valor|
    |---|---|
    |Endpoint | Nuevo |
    |Nombre del Endpoint | escriba un nombre único|
    |Nombre de implementación | escriba un nombre único |
    |Máquina virtual | Standard_DS3_v2 (o la que se encuentre disponible en la regiónO |
    |Número de instancias| 1 |
    |Inferencia de la recopilación de datos | deshabilitada |

>Configuración avanzada : Usar la configuración predeterminada
<img width="390" height="106" alt="Captura desde 2025-10-14 16-15-08" src="https://github.com/user-attachments/assets/6cec8d27-e88e-4143-9d39-89cdcfb91319" />
<img width="1268" height="918" alt="Captura desde 2025-10-14 16-17-28" src="https://github.com/user-attachments/assets/9c594330-f07c-41d5-958c-8c2cd980fbc2" />

2. En el portal de Azure AI Foundry, en el panel de navegación, en la sección Mis recursos, seleccione la página Modelos + Endpoints, si se abre la página para el modelo gpt-4o, utilice su botón Atrás para ver todos los modelos y endpoints.
3. Inicialmente, la página puede mostrar solo las implementaciones de modelos. Puede pasar algún tiempo antes de que se muestre la implementación e incluso más tiempo antes de que se cree correctamente.
<img width="1269" height="514" alt="Captura desde 2025-10-14 16-20-14" src="https://github.com/user-attachments/assets/dcd5d091-c79c-4080-8dd3-c04301135311" />

4. Cuando la implementación se haya realizado correctamente, selecciónela. Luego, vea su página de prueba.
<img width="1540" height="901" alt="Captura desde 2025-10-14 16-29-14" src="https://github.com/user-attachments/assets/3db27ecc-683a-4d09-ae4c-da1676b945cd" />

5. Ingrese el mensaje y revise la respuesta What is there to do in San Francisco?

6. Ingrese el mensaje y revise la respuesta Tell me something about the history of the city.

El panel de prueba debe tener un aspecto similar al siguiente:

<img width="1559" height="898" alt="Captura desde 2025-10-14 16-32-48" src="https://github.com/user-attachments/assets/07fc8602-bd03-42f4-9746-9523c0b4fe89" />

7. Vea la página Consumir del Endpoint y tenga en cuenta que contiene información de conexión y código de ejemplo que puede usar para crear una aplicación cliente para el endpoint, lo que le permite integrar la solución de Prompt Flow en una aplicación como una aplicación de IA generativa.

<img width="1066" height="879" alt="Captura desde 2025-10-14 16-33-28" src="https://github.com/user-attachments/assets/ec86e11a-9765-4a69-8837-0d1d1ce7a7ff" />

## Limpiar
Limpiar
Cuando termine de explorar el Prompt Flow, debe eliminar los recursos que ha creado para evitar costos innecesarios de Azure.<br>
Vaya a Azure Portal en https://portal.azure.com<br>
En Azure Portal, en la página principal, seleccione Grupos de recursos.<br>
Seleccione el grupo de recursos que creó para este ejercicio.<br>
En la parte superior de la página Información general del grupo de recursos, seleccione Eliminar grupo de recursos.<br>
Escriba el nombre del grupo de recursos para confirmar que desea eliminarlo y seleccione Eliminar.


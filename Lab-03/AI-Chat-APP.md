 # Crear una aplicación de chat de IA generativa

 En este ejercicio, usará el SDK de Python de Azure AI Foundry para crear una aplicación de chat sencilla que se conecte a un proyecto y chatee con un modelo de lenguaje.

 ## Implementación de un modelo en un proyecto de Azure AI Foundry

 1. En un explorador web, abra el portal de Azure AI Foundry e inicie sesión con sus credenciales de Azure. Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto):https://ai.azure.com
 <img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/28928bf2-e6c7-4f37-bb4e-d152f2186a2a" />

 2. En la página de inicio, en la sección Explorar modelos y capacidades, busque el modelo; que usaremos en nuestro proyecto.gpt-4o
 <img width="1784" height="937" alt="Captura desde 2025-10-10 10-58-37" src="https://github.com/user-attachments/assets/133c941d-4585-4a40-aaf0-244a89c2390a" />

 3. En los resultados de la búsqueda, seleccione el modelo gpt-4o para ver sus detalles y, a continuación, en la parte superior de la página del modelo, seleccione Usar este modelo.
 <img width="528" height="460" alt="Captura desde 2025-10-10 10-59-19" src="https://github.com/user-attachments/assets/c871b00f-0c3e-4ba5-bb71-2fa8c0c0eec5" />

 4. Cuando se le pida que cree un proyecto, introduzca un nombre válido para el proyecto y expanda Opciones avanzadas
 5. Seleccione Personalizar y especifique la siguiente configuración para el proyecto:
    
    |Configuración|Valor|
    |---|---|
    |Recurso de Azure AI Foundry| un nombre válido para el recurso de Azure AI Foundry |
    |Suscripción| su suscripción de Azure |
    |Grupo de recursos| crear o seleccionar un grupo de recursos |
    |Región| seleccione cualquier AI Foundry recomendado |
 <img width="660" height="713" alt="Captura desde 2025-10-10 11-01-09" src="https://github.com/user-attachments/assets/772b2e94-317b-4311-bf3f-817c1bd40fe8" />

6. Seleccione Crear y espere a que se cree el proyecto, incluida la implementación del modelo gpt-4o que seleccionó.
> **Importante**: En función de la cuota disponible para los modelos gpt-4o, es posible que reciba un mensaje adicional para implementar el modelo en un recurso de una región diferente. Si esto sucede, hágalo, utilizando la configuración predeterminada. Más adelante en el ejercicio, no podrá usar el endpoint predeterminado del proyecto: debe usar el URI de destino específico del modelo.
>
> <img width="688" height="910" alt="Captura desde 2025-10-10 11-04-19" src="https://github.com/user-attachments/assets/27484b81-5bd9-4f0c-9c1f-cfdcc9a04b46" />

7. Cuando se crea el proyecto, el Playground de chat se abrirá automáticamente para que pueda probar el modelo (si no es así, en el panel de tareas de la izquierda, seleccione Playgrounds y, a continuación, abra el Playground de Chat).
 <img width="574" height="248" alt="Captura desde 2025-10-10 11-07-21" src="https://github.com/user-attachments/assets/01b0d8ed-59ea-4a50-9148-18f2f2a5f30d" />

8. En el panel Configuración, anote el nombre de la implementación del modelo; que debería ser gpt-4o. Para confirmarlo, vea la implementación en la página Modelos y endpoints (solo tiene que abrir esa página en el panel de navegación de la izquierda).
<img width="230" height="147" alt="Captura desde 2025-10-10 12-49-52" src="https://github.com/user-attachments/assets/0f04ec0f-c88d-433d-baf1-f3e43819434b" />

##  Creación de una aplicación cliente para chatear con el modelo
Ahora que ha implementado un modelo, puede usar los SDK de Azure AI Foundry y Azure OpenAI para desarrollar una aplicación que chatee con él.

1. En el portal de Azure AI Foundry, consulte la página Información general del proyecto.
2. En el área Endpoints y claves, asegúrese de que la biblioteca de Azure AI Foundry está seleccionada y vea el Endpoint del proyecto de Azure AI Foundry. Usará este Endpoint para conectarse al proyecto y al modelo en una aplicación cliente.
<img width="714" height="393" alt="Captura desde 2025-10-10 11-09-16" src="https://github.com/user-attachments/assets/27d35b02-27c9-40ba-b6c2-c2ed264145a7" />

>**Nota**: También puede usar el endpoint de Azure OpenAI.

3. Abra una nueva pestaña del explorador (manteniendo abierto el portal de Azure AI Foundry en la pestaña existente). A continuación, en la nueva pestaña, vaya a Azure Portal en ; iniciar sesión con sus credenciales de Azure si se le solicita.https://portal.azure.com
  Cierre las notificaciones de bienvenida para ver la página principal de Azure Portal.
4. Use el botón [>_] situado a la derecha de la barra de búsqueda en la parte superior de la página para crear una nueva instancia de Cloud Shell en Azure Portal y seleccione un entorno de PowerShell sin almacenamiento en la suscripción.
<img width="275" height="190" alt="Captura desde 2025-10-10 11-11-04" src="https://github.com/user-attachments/assets/c740f973-123c-414d-a1bc-761c6eedd1b7" />

Cloud Shell proporciona una interfaz de línea de comandos en un panel en la parte inferior de Azure Portal. Puede cambiar el tamaño o maximizar este panel para que sea más fácil trabajar en él.
<img width="1172" height="475" alt="Captura desde 2025-10-10 11-12-14" src="https://github.com/user-attachments/assets/1f836041-c34e-44f4-bc92-5b6e07355577" />

5. En la barra de herramientas de Cloud Shell, en el menú Configuración, seleccione Ir a la versión clásica (esto es necesario para usar el editor de código).
  <img width="503" height="431" alt="Captura desde 2025-10-10 11-16-06" src="https://github.com/user-attachments/assets/6c27c23f-7857-4b33-a2df-5b5d2bd13de2" />

>**Importante**: Asegúrese de haber cambiado a la versión clásica de Cloud Shell antes de continuar.

6. En el panel de Cloud Shell, escriba los siguientes comandos para clonar el repositorio de GitHub que contiene los archivos de código para este ejercicio (escriba el comando o cópielo en el portapapeles y, a continuación, haga clic con el botón derecho en la línea de comandos y péguelo como texto sin formato):

>rm -r mslearn-ai-foundry -f  
>git clone https://github.com/microsoftlearning/mslearn-ai-studio mslearn-ai-foundry
<img width="998" height="274" alt="Captura desde 2025-10-10 11-18-51" src="https://github.com/user-attachments/assets/26bbd2ec-b936-4631-a2bb-b35f0eeb629a" />


7. Una vez clonado el repositorio, vaya a la carpeta que contiene los archivos de código de la aplicación de chat y véalos:
>cd mslearn-ai-foundry/labfiles/chat-app/python  
>ls -a -l

<img width="653" height="196" alt="Captura desde 2025-10-10 11-20-01" src="https://github.com/user-attachments/assets/b5cc7b7c-dbf9-4c31-9258-ab2e7908a95c" /> <br>
La carpeta contiene un archivo de código, así como un archivo de configuración para la configuración de la aplicación y un archivo que define el tiempo de ejecución del proyecto y los requisitos del paquete.

8. En el panel de la línea de comandos de Cloud Shell, escriba el siguiente comando para instalar las bibliotecas que usará:
>python -m venv labenv  
>./labenv/bin/Activate.ps1  
>pip install -r requirements.txt azure-identity azure-ai-projects openai
<img width="1311" height="166" alt="Captura desde 2025-10-10 11-23-42" src="https://github.com/user-attachments/assets/da0cb924-8e73-4741-bfe4-41481f11e973" />

9. Introduzca el siguiente comando para editar el archivo de configuración que se ha proporcionado:
>code .env
<img width="739" height="76" alt="Captura desde 2025-10-10 11-24-18" src="https://github.com/user-attachments/assets/304e1747-f4d6-4539-9e43-45930ea2b456" />

El archivo se abre en un editor de código.  

10. En el archivo de código, reemplace el marcador de posición your_project_endpoint por el Endpoint adecuado para el modelo y el marcador de posición your_model_deployment por el nombre asignado a la implementación del modelo gpt-4o.
    
>**Importante**: Si implementó el modelo gpt-4o en la región predeterminada del proyecto, puede usar el proyecto de Azure AI Foundry o el Endpoint de Azure OpenAI en la página Información general del proyecto para conectarse al modelo. Si no tenía una cuota suficiente e implementó el modelo en otra región, en la página Modelos + puntos de conexión, seleccione el modelo y use el URI de destino para el modelo.  
>Para este ejercicio, utilicé el URI de destino para el modelo, ya que el modelo se implemento en otra ubicación
<img width="742" height="132" alt="Captura desde 2025-10-10 11-24-40" src="https://github.com/user-attachments/assets/cf93d0cc-9252-4afb-9805-30f341b68924" />
<img width="591" height="296" alt="Captura desde 2025-10-10 12-50-18" src="https://github.com/user-attachments/assets/08fe9fb6-9cc2-4093-ab5d-2dda18c3fe40" />

11.Después de reemplazar los marcadores de posición, dentro del editor de código, use el comando CTRL+S o haga clic con el botón derecho > Guardar para guardar los cambios y, a continuación, use el comando CTRL+Q o haga clic con el botón derecho > Salir para cerrar el editor de código mientras mantiene abierta la línea de comandos del shell en la nube

<img width="145" height="164" alt="Captura desde 2025-10-10 11-44-48" src="https://github.com/user-attachments/assets/698582b6-072a-4f95-9eac-6a62757768f5" />

<img width="247" height="208" alt="Captura desde 2025-10-10 11-44-22" src="https://github.com/user-attachments/assets/222d6e2a-c5e3-4837-8282-69621dbfd12d" />

## Escribir código para conectarse al proyecto y chatear con el modelo

>**Sugerencia**: A medida que agrega código, asegúrese de mantener la sangría correcta.

1. Escriba el siguiente comando para editar el archivo de código que se ha proporcionado:
>code chat-app.py
<img width="835" height="59" alt="Captura desde 2025-10-10 11-45-54" src="https://github.com/user-attachments/assets/e843b992-f19e-42da-82a1-f18e4b613422" />
<img width="1025" height="624" alt="Captura desde 2025-10-10 11-49-26" src="https://github.com/user-attachments/assets/9d47e408-fad7-42ec-84b7-27a48ab31ca6" />

2. En el archivo de código, anote las instrucciones existentes que se han agregado en la parte superior del archivo para importar los espacios de nombres del SDK necesarios. A continuación, busque el comentario *Add references*  y agregue el código siguiente para hacer referencia a los espacios de nombres en las bibliotecas que instaló anteriormente:
 
>from azure.identity import DefaultAzureCredential  
>from azure.ai.projects import AIProjectClient
>from openai import AzureOpenAI
<img width="614" height="186" alt="Captura desde 2025-10-10 11-49-52" src="https://github.com/user-attachments/assets/2986b90b-5c71-4e9f-81e6-0bd8f69b885f" />

3.En la función main, en el comentario *Get Configuration settings*, tenga en cuenta que el código carga la cadena de conexión del proyecto y los valores del nombre de implementación del modelo que definió en el archivo de configuración.
<img width="653" height="341" alt="Captura desde 2025-10-10 11-51-00" src="https://github.com/user-attachments/assets/4865c8e0-595e-42b8-9496-a4c8eb662c00" />

4.Busque el comentario *Initialize the project client* y agregue el código siguiente para conectarse al proyecto de Azure AI Foundry:
>**Sugerencia**: Tenga cuidado de mantener el nivel de sangría correcto para el código.


>project_client = AIProjectClient(            
>         credential=DefaultAzureCredential(  
>             exclude_environment_credential=True,  
>             exclude_managed_identity_credential=True  
>         ),  
>         endpoint=project_endpoint,  
>     )  

<img width="702" height="341" alt="Captura desde 2025-10-10 11-53-48" src="https://github.com/user-attachments/assets/b093bd02-8e78-427f-ac90-a040a63463d4" />

5. Busque el comentario *Get a chat client* y agregue el código siguiente para crear un objeto de cliente para chatear con un modelo, puede variar la api_version:
>openai_client = project_client.get_openai_client(api_version="2024-10-21")

<img width="881" height="172" alt="Captura desde 2025-10-10 11-56-17" src="https://github.com/user-attachments/assets/7e4fb1f8-c95d-4be5-b0cd-dd96408d8657" />

6. Busque el comentario *Initialize prompt with system message* y agregue el código siguiente para inicializar una colección de mensajes con un mensaje del sistema.
>prompt = [
>{"role": "system", "content": "You are a helpful AI assistant that answers questions."}
> ]

<img width="922" height="163" alt="Captura desde 2025-10-10 11-58-53" src="https://github.com/user-attachments/assets/e733e0e6-dc90-43b2-8ed2-30dddb120ab7" />

7. Tenga en cuenta que el código incluye un bucle para permitir que un usuario ingrese un mensaje hasta que ingrese "salir". A continuación, en la sección de bucle, busque el comentario *Get a chat completion* y agregue el código siguiente para agregar la entrada del usuario al mensaje, recuperar la finalización del modelo y agregar la finalización al mensaje (para conservar el historial de chat para futuras iteraciones):
>prompt.append({"role": "user", "content": input_text})  
>response = openai_client.chat.completions.create(  
>  model=model_deployment,  
>  messages=prompt)
>completion = response.choices[0].message.content  
>print(completion)
>prompt.append({"role": "assistant", "content": completion})
<img width="631" height="322" alt="Captura de pantalla 2025-10-13 171905" src="https://github.com/user-attachments/assets/484353f4-a14d-4f8a-9db0-5107c0a003dd" />



8. Use el comando CTRL+S para guardar los cambios en el archivo de código.

## Inicie sesión en Azure y ejecute la aplicación

1. En el panel de línea de comandos de Cloud Shell, escriba el siguiente comando para iniciar sesión en Azure.
> az login
<img width="877" height="94" alt="Captura desde 2025-10-10 12-15-06" src="https://github.com/user-attachments/assets/47d94801-6ef9-4da7-adb4-4892857b55a4" />

2. Cuando se le solicite, siga las instrucciones para abrir la página de inicio de sesión en una nueva pestaña y escriba el código de autenticación proporcionado y sus credenciales de Azure. A continuación, complete el proceso de inicio de sesión en la línea de comandos, seleccionando la suscripción que contiene el centro de Azure AI Foundry si se le solicita.
<img width="1574" height="190" alt="Captura desde 2025-10-10 12-15-52" src="https://github.com/user-attachments/assets/a86cf6c5-7778-4b6d-8c9f-f702649d25fb" />

3. Después de iniciar sesión, escriba el siguiente comando para ejecutar la aplicación:
>python chat-app.py
<img width="982" height="66" alt="Captura desde 2025-10-10 12-16-31" src="https://github.com/user-attachments/assets/8f3caa42-800e-4205-b89a-41238188bf61" />

4. Cuando se le solicite, ingrese una pregunta, como y revise la respuesta de su modelo de IA generativa.What is the fastest animal on Earth?
<img width="1785" height="191" alt="Captura desde 2025-10-10 12-23-30" src="https://github.com/user-attachments/assets/8fe300d7-f276-4220-afed-16f0daac2cf0" />

5. Pruebe algunas preguntas de seguimiento. La conversación debe continuar, utilizando el historial de chat como contexto para cada iteración.Where can I see one?Are they endangered?
6. Cuando haya terminado, ingrese para salir del programa quit

## Resumen
En este ejercicio, usó el SDK de Azure AI Foundry para crear una aplicación cliente para un modelo de IA generativa que implementó en un proyecto de Azure AI Foundry.

## Limpiar
Si ha terminado de explorar el portal de Azure AI Foundry, debe eliminar los recursos que ha creado en este ejercicio para evitar incurrir en costos innecesarios de Azure.

Abra Azure Portal y vea el contenido del grupo de recursos donde implementó los recursos usados en este ejercicio.
En la barra de herramientas, seleccione Eliminar grupo de recursos.
Escriba el nombre del grupo de recursos y confirme que desea eliminarlo.








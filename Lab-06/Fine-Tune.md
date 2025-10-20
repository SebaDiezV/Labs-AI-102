# Ajuste de un modelo de lenguaje
Cuando desee que un modelo de lenguaje se comporte de una manera determinada, puede usar la ingeniería de prompt para definir el comportamiento deseado. Cuando desee mejorar la coherencia del comportamiento deseado, puede optar por ajustar un modelo, comparándolo con su enfoque de ingeniería de prompt para evaluar qué método se adapta mejor a sus necesidades.

En este ejercicio, ajustará un modelo de lenguaje con Azure AI Foundry que desea usar para un escenario de aplicación de chat personalizado. Comparará el modelo ajustado con un modelo base para evaluar si el modelo ajustado se ajusta mejor a sus necesidades.

Imagina que trabajas para una agencia de viajes y estás desarrollando una aplicación de chat para ayudar a las personas a planificar sus vacaciones. El objetivo es crear un chat simple e inspirador que sugiera destinos y actividades con un tono conversacional consistente y amigable.

## Implementación de un modelo en un proyecto de Azure AI Foundry

Empecemos por implementar un modelo en un proyecto de Azure AI Foundry.

1. En un explorador web, abra el portal de Azure AI Foundry (https://ai.azure.com) e inicie sesión con sus credenciales de Azure. Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto)
<img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/cf1859c5-fa04-42bb-b458-18a6571d440c" />

2. En la página de inicio, en la sección Explorar modelos y capacidades, busque el modelo gpt-4o que usaremos en nuestro proyecto.
<img width="1162" height="426" alt="Captura desde 2025-10-17 13-17-22" src="https://github.com/user-attachments/assets/bf8e7341-99ff-4e7b-9d52-3be096f426db" />

3. En los resultados de la búsqueda, seleccione el modelo gpt-4o para ver sus detalles y, a continuación, en la parte superior de la página del modelo, seleccione Usar este modelo.
<img width="1175" height="489" alt="Captura desde 2025-10-17 13-17-45" src="https://github.com/user-attachments/assets/fb58873c-2cb8-451f-802b-f7bf95bbc443" />

4. Cuando se le pida que cree un proyecto, introduzca un nombre válido para el proyecto y expanda Opciones avanzadas.
5. Seleccione Personalizar y especifique la siguiente configuración para el proyecto
   
      |Configuración|Valor|
      |---|---|   
      |Suscripción | su suscripción de Azure|
      |Grupo de recursos | crear o seleccionar un grupo de recursos|
      |Región | Este de EE. UU. 2, Centro-norte de EE. UU. o Centro de Suecia|
   
<img width="664" height="690" alt="Captura desde 2025-10-17 13-22-17" src="https://github.com/user-attachments/assets/95a710be-6f36-4836-b9cd-94550657e3c8" />

6. Seleccione Crear y espere a que se cree el proyecto. Si se le solicita, implemente el modelo gpt-4o con el tipo de implementación estándar global y personalice los detalles de la implementación para establecer un límite de velocidad de tokens por minuto de 50 K (o el máximo disponible si es inferior a 50 K).
<img width="681" height="907" alt="Captura desde 2025-10-17 13-24-08" src="https://github.com/user-attachments/assets/2d870e44-d74d-4af0-bc43-e6e6899dbd07" />

7. Cuando se crea tu proyecto, el chat playground se abrirá automáticamente para que puedas probar tu modelo

8. En el panel Configuración, anote el nombre de la implementación del modelo; que debería ser GPT-4o. Para confirmarlo, vea la implementación en la página Modelos y endpoints (solo tiene que abrir esa página en el panel de navegación de la izquierda).
<img width="591" height="255" alt="Captura desde 2025-10-17 13-26-09" src="https://github.com/user-attachments/assets/c0807c67-c8ae-4e74-a083-0b93d6c25cd6" />

9. En el panel de navegación de la izquierda, seleccione Información general para ver la página principal del proyecto; que se ve así:
<img width="1085" height="418" alt="Captura desde 2025-10-17 13-26-45" src="https://github.com/user-attachments/assets/bfd1396e-1824-4fe6-86b2-50febe46d5e7" />

## Ajuste de un modelo

Dado que el ajuste de un modelo tarda algún tiempo en completarse, comenzará el trabajo de ajuste ahora y volverá a él después de explorar el modelo gpt-4o base que ya implementó.

1. Descargue el conjunto de datos de entrenamiento en y guárdelo como un archivo JSONL localmente.
   
       https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-studio/refs/heads/main/data/travel-finetune-hotel.jsonl
> **Nota**: Es posible que tu dispositivo guarde el archivo de forma predeterminada como un archivo .txt. Seleccione todos los archivos y quite el sufijo .txt para asegurarse de que está guardando el archivo como JSONL.

2. Vaya a la página Fine-Tuning en la sección Build and Customize, utilizando el menú de la izquierda.
<img width="226" height="219" alt="Captura desde 2025-10-17 13-30-17" src="https://github.com/user-attachments/assets/cfa45f4c-9f67-4835-90fd-dce243658e4f" />

3. Seleccione el botón para agregar un nuevo modelo de Fine-tune, seleccione el modelo gpt-4o y, a continuación, seleccione Siguiente.
<img width="222" height="188" alt="Captura desde 2025-10-17 13-35-25" src="https://github.com/user-attachments/assets/deeee062-b546-4faf-81ce-0cf00a2cbba8" />

<img width="1046" height="841" alt="Captura desde 2025-10-17 13-36-21" src="https://github.com/user-attachments/assets/e0762232-25e0-4dbd-baa3-655c408b6f73" />

4. Ajuste el modelo mediante la siguiente configuración
   
    |Configuración|Valor|
    |---|---|   
    |Método de personalización | Supervisado|
    |Modelo base | seleccione la versión predeterminada de gpt-4o|
    |Datos de entrenamiento | seleccione la opción Agregar datos de entrenamiento y cargue y aplique el archivo .jsonl que descargó anteriormente|
    |Sufijo del modelo| ft-travel|
    |Semilla | *Aleatorio|

<img width="651" height="859" alt="Captura desde 2025-10-17 13-37-24" src="https://github.com/user-attachments/assets/0a42f845-c3e3-44a3-ad57-054015fc66c8" />
<img width="651" height="859" alt="Captura desde 2025-10-17 13-38-12" src="https://github.com/user-attachments/assets/204c225f-798f-45ad-b2c0-b5a8cc83ef94" />
<img width="651" height="859" alt="Captura desde 2025-10-17 13-39-13" src="https://github.com/user-attachments/assets/921d7df1-7d1c-4aec-a90d-20651fe31bdb" />

5. Envíe los detalles de ajuste y se iniciará el trabajo. Puede tardar algún tiempo en completarse. Puede continuar con la siguiente sección del ejercicio mientras espera
>**Nota**: El ajuste y la implementación pueden tardar una cantidad significativa de tiempo (30 minutos o más), por lo que es posible que deba volver a consultar periódicamente.
>Para ver más detalles del progreso hasta el momento, seleccione el trabajo de ajuste del modelo y vea su pestaña Registros.

## Chatear con un modelo base
Mientras espera a que se complete el trabajo de ajuste, hablemos con un modelo GPT 4o base para evaluar su rendimiento.

1. En el panel de navegación de la izquierda, seleccione Playgrounds y abra el playground de Chat.
<img width="229" height="163" alt="Captura desde 2025-10-17 13-40-22" src="https://github.com/user-attachments/assets/86e095e6-a49e-41d3-98d0-8f8e234fae59" />

2. Compruebe que el modelo base gpt-4o desplegado está seleccionado en el panel de configuración.

3. En la ventana de chat, ingrese la consulta *What can you do?* y vea la respuesta.
<img width="1532" height="858" alt="Captura desde 2025-10-17 13-41-39" src="https://github.com/user-attachments/assets/9f1bed65-7dc9-47fa-9b5b-6ca0d56c9231" />

Las respuestas pueden ser bastante genéricas. Recuerda que queremos crear una aplicación de chat que inspire a las personas a viajar.

4. Actualice el mensaje del sistema en el panel de configuración con el siguiente mensaje:

        You are an AI assistant that helps people plan their travel.
<img width="553" height="460" alt="Captura desde 2025-10-17 13-42-27" src="https://github.com/user-attachments/assets/4197c539-b346-4acd-bf82-31e961b8bb78" />

5. Seleccione Aplicar cambios para actualizar el mensaje del sistema.

6. En la ventana de chat, vuelva a escribir la consulta *What can you do?* y vea la respuesta.  Como respuesta, el asistente puede decirle que puede ayudarlo a reservar vuelos, hoteles y autos de alquiler para su viaje. Desea evitar este comportamiento.
<img width="962" height="754" alt="Captura desde 2025-10-17 13-43-34" src="https://github.com/user-attachments/assets/9599fbbb-bce5-4c11-9794-43fe76ba0f49" />

7. Actualice el mensaje del sistema nuevamente con un nuevo mensaje:

       You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms.
       You should not provide any hotel, flight, rental car or restaurant recommendations.
       Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday.

<img width="557" height="502" alt="Captura desde 2025-10-17 13-44-08" src="https://github.com/user-attachments/assets/b88f4d40-bbdc-48fd-b0cc-dd3f6212c8da" />


8. Continúe probando su aplicación de chat para verificar que no proporcione ninguna información que no se base en los datos recuperados. Por ejemplo, haga las siguientes preguntas y revise las respuestas del modelo, prestando especial atención al tono y al estilo de escritura que el modelo usa para responder:

        Where in Rome should I stay?

        I'm mostly there for the food. Where should I stay to be within walking distance of affordable restaurants?

        What are some local delicacies I should try?

        When is the best time of year to visit in terms of the weather?

        What's the best way to get around the city

<img width="914" height="499" alt="Captura desde 2025-10-17 13-44-57" src="https://github.com/user-attachments/assets/e4576437-1a3a-4e5f-9626-95bc9341d2b3" />

## Revisar el archivo de entrenamiento

El modelo base parece funcionar bastante bien, pero es posible que esté buscando un estilo conversacional particular de su aplicación de IA generativa. Los datos de entrenamiento utilizados para el ajuste le ofrecen la oportunidad de crear ejemplos explícitos de los tipos de respuesta que desea.

1. Abra el archivo JSONL que descargó anteriormente (puede abrirlo en cualquier editor de texto)
2. Examine la lista de documentos JSON en el archivo de datos de entrenamiento. El primero debe ser similar a este (formateado para facilitar la lectura):

       {"messages": [
           {"role": "system", "content": "You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms. You should not provide any hotel, flight, rental car or restaurant recommendations. Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday."},
           {"role": "user", "content": "What's a must-see in Paris?"},
           {"role": "assistant", "content": "Oh la la! You simply must twirl around the Eiffel Tower and snap a chic selfie! After that, consider visiting the Louvre Museum to see the Mona Lisa and other masterpieces. What type of attractions are you most interested in?"}
         ]}
Cada interacción de ejemplo de la lista incluye el mismo mensaje del sistema que probó con el modelo base, un mensaje de usuario relacionado con una consulta de viaje y una respuesta. El estilo de las respuestas en los datos de entrenamiento ayudará al modelo ajustado a aprender cómo debe responder.

## Implementación del modelo ajustado

Cuando el ajuste se haya completado correctamente, puede implementar el modelo ajustado.

1. Vaya a la página Fine-tuning en Build and customize para encontrar el trabajo de Fine-tune y su estado. Si todavía se está ejecutando, puede optar por continuar chateando con el modelo base implementado o tomar un descanso. Si se completa, puede continuar.
<img width="956" height="343" alt="Captura desde 2025-10-17 13-49-39" src="https://github.com/user-attachments/assets/6c2c10dc-b526-447c-81da-a57f90531337" />
<img width="956" height="343" alt="Captura desde 2025-10-17 15-45-49" src="https://github.com/user-attachments/assets/23411a66-7036-4453-8550-beaca8624c1f" />

> **Sugerencia**: Utilice el botón Actualizar de la página de ajuste para actualizar la vista. Si el trabajo de ajuste desaparece por completo, actualice la página en el navegador.

2. Seleccione el vínculo del trabajo de ajuste para abrir su página de detalles. A continuación, seleccione la pestaña Métricas y explore las métricas de ajuste.
<img width="1556" height="869" alt="Captura desde 2025-10-17 15-46-09" src="https://github.com/user-attachments/assets/d4d46cbb-4426-421e-8921-ce42cd49883c" />

3. Implemente el modelo ajustado con las siguientes configuraciones:

    |Configuración|Valor|
    |---|---|   
    |Nombre de implementación | un nombre válido para la implementación del modelo|
    |Tipo de implementación | Estándar|
    |Límite de velocidad de tokens por minuto (miles) | 50K (o el máximo disponible en su suscripción si es inferior a 50K)|
    |Filtro de contenido | Predeterminado|

<img width="373" height="264" alt="Captura desde 2025-10-17 15-46-36" src="https://github.com/user-attachments/assets/3f56df0b-7364-49d1-bc82-b41942fe22fa" />
<img width="645" height="894" alt="Captura desde 2025-10-17 15-47-39" src="https://github.com/user-attachments/assets/db66082f-7b16-4b84-a087-6df03bc7d6c6" />

4. Espere a que se complete la implementación antes de poder probarla, esto puede tardar un tiempo. Compruebe el estado de aprovisionamiento hasta que se haya realizado correctamente (es posible que tenga que actualizar el explorador para ver el estado actualizado).

<img width="910" height="384" alt="Captura desde 2025-10-17 15-50-12" src="https://github.com/user-attachments/assets/885f6b10-f4ea-4c6c-997a-1d4d891d02e8" />
<img width="910" height="384" alt="Captura desde 2025-10-17 16-21-33" src="https://github.com/user-attachments/assets/e4e11e54-081f-4b4f-b588-d2373b06ce2a" />


## Prueba del modelo ajustado

Ahora que implementó el modelo ajustado, puede probarlo como probó el modelo base implementado.

1. Cuando la implementación esté lista, vaya al modelo ajustado y seleccione Abrir en playground.

2. Asegúrese de que el mensaje del sistema incluya estas instrucciones:

       You are an AI travel assistant that helps people plan their trips. Your objective is to offer support for travel-related inquiries, such as visa requirements, weather forecasts, local attractions, and cultural norms.
       You should not provide any hotel, flight, rental car or restaurant recommendations.
       Ask engaging questions to help someone plan their trip and think about what they want to do on their holiday.

<img width="563" height="593" alt="Captura desde 2025-10-17 15-48-32" src="https://github.com/user-attachments/assets/69a91103-d8a3-47a0-9bb2-8c3809510f82" />

3. Pruebe el modelo ajustado para evaluar si su comportamiento es más coherente ahora. Por ejemplo, vuelva a hacer las siguientes preguntas y explore las respuestas del modelo:

        Where in Rome should I stay?

        I'm mostly there for the food. Where should I stay to be within walking distance of affordable restaurants?

        What are some local delicacies I should try?

        When is the best time of year to visit in terms of the weather?

        What's the best way to get around the city

   
<img width="997" height="711" alt="Captura desde 2025-10-17 16-24-42" src="https://github.com/user-attachments/assets/091e6310-ee39-4140-a05e-fdacbb48b53f" />

4. Después de revisar las respuestas, ¿cómo se comparan con las del modelo base?

## Limpiar

Si ha terminado de explorar Azure AI Foundry, debe eliminar los recursos que ha creado para evitar costos innecesarios de Azure.

* Vaya a Azure Portal en https://portal.azure.com
* En Azure Portal, en la página principal, seleccione Grupos de recursos.
* Seleccione el grupo de recursos que creó para este ejercicio.
* En la parte superior de la página Información general del grupo de recursos, seleccione Eliminar grupo de recursos.
* Escriba el nombre del grupo de recursos para confirmar que desea eliminarlo y seleccione Eliminar.

>**Nota**: si no puedes borrar el recurso por algun error, ingresa AI Foundry y elimina el modelo entrenado con Fine-tune de forma manual.
>Elimina el grupo de recursos nuevamente

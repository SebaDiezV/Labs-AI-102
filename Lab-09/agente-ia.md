# Explora el desarrollo de agentes de IA

En este ejercicio, usará el servicio Azure AI Agent en el portal de Azure AI Foundry para crear un agente de IA sencillo que ayude a los empleados con las reclamaciones de gastos.

## Creación de un proyecto y un agente de Azure AI Foundry

1. En un explorador web, abra el portal de Azure AI Foundry (https://ai.azure.com) e inicie sesión con sus credenciales de Azure. Cierre los paneles de sugerencias o de inicio rápido que se abran la primera vez que inicie sesión y, si es necesario, use el logotipo de Azure AI Foundry en la parte superior izquierda para navegar a la página principal, que tiene un aspecto similar al de la imagen siguiente (cierre el panel Ayuda si está abierto)
   <img width="1887" height="861" alt="Captura de pantalla 2025-10-13 151024" src="https://github.com/user-attachments/assets/28928bf2-e6c7-4f37-bb4e-d152f2186a2a" />

2. En la página de inicio, seleccione Crear un agente.
   <img width="627" height="364" alt="Captura desde 2025-10-24 11-01-07" src="https://github.com/user-attachments/assets/b165fa93-971d-471d-9d0a-66f7f41bc553" />

3. Cuando se le pida que cree un proyecto, escriba un nombre válido para el proyecto.

4. Expanda Opciones avanzadas y especifique la siguiente configuración:

    |Configuración|Valor|
    |---|---|
    |Recurso de Azure AI Foundry| un nombre válido para el recurso de Azure AI Foundry |
    |Suscripción| su suscripción de Azure |
    |Grupo de recursos| crear o seleccionar un grupo de recursos |
    |Región| seleccione cualquier AI Foundry recomendado |

   <img width="661" height="719" alt="Captura desde 2025-10-24 11-02-54" src="https://github.com/user-attachments/assets/0cab96d8-4fa9-4ea6-82dd-4dd8285f0d36" />

5. Seleccione Crear y espere a que se cree el proyecto.

6. Si se le solicita, implemente un modelo gpt-4o con el tipo de implementación Global Standard o Standard (según la disponibilidad de cuota) y personalice los detalles de la implementación para establecer un límite de velocidad de tokens por minuto de 50 K (o el máximo disponible si es inferior a 50 K).
   <img width="1059" height="811" alt="Captura desde 2025-10-24 11-05-53" src="https://github.com/user-attachments/assets/9ac54c09-dec1-4099-a68c-17cce371138f" />
   <img width="665" height="912" alt="Captura desde 2025-10-24 11-06-19" src="https://github.com/user-attachments/assets/efc76eba-82ba-4e2d-8e23-7fbf7b69440c" />


7. Cuando se crea tu proyecto, el playground de Agentes se abrirá automáticamente para que puedas seleccionar o implementar un modelo:
   <img width="1773" height="875" alt="Captura desde 2025-10-24 11-07-04" src="https://github.com/user-attachments/assets/7fab7ee7-0b7c-46c6-98a2-64e4d49b64fb" />

   Verá que se ha creado un agente con un nombre predeterminado para usted, junto con la implementación del modelo base.

## Crea tu agente

Ahora que ha implementado un modelo, está listo para crear un agente de IA. En este ejercicio, creará un agente simple que responda preguntas basadas en una política de gastos corporativos. Descargará el documento de la política de gastos y lo utilizará como datos de conexión a tierra para el agente.

1. Abra otra pestaña del navegador, descargue Expenses_policy.docx y guárdela localmente. Este documento contiene detalles de la directiva de gastos de la corporación ficticia de Contoso.

        https://raw.githubusercontent.com/MicrosoftLearning/mslearn-ai-agents/main/Labfiles/01-agent-fundamentals/Expenses_Policy.docx

2. Regrese a la pestaña del navegador que contiene el playground de Foundry Agents y busque el panel Configuración (puede estar al lado o debajo de la ventana de chat).

3. Establezca el nombre del agente como ExpensesAgent, asegúrese de que la implementación del modelo gpt-4o que creó anteriormente esté seleccionada y establezca las instrucciones:

        You are an AI assistant for corporate expenses.
        You answer questions about expenses based on the expenses policy data.
        If a user wants to submit an expense claim, you get their email address, a description of the claim, and the amount to be claimed and write the claim details to a text file that the user can download.

   <img width="496" height="860" alt="Captura desde 2025-10-24 11-13-41" src="https://github.com/user-attachments/assets/d50bcd81-e2b0-4cb1-9e54-37086726771f" />

4. Más abajo en el panel Configuración, junto al encabezado Knowledge, seleccione + Agregar. A continuación, en el cuadro de diálogo Agregar conocimiento, seleccione Archivos.<br>
   <img width="470" height="166" alt="Captura desde 2025-10-24 11-14-08" src="https://github.com/user-attachments/assets/64c55829-6a64-497b-aae2-123342606923" />

5. En el cuadro de diálogo Agregar archivos, cree un nuevo almacén de vectores denominado Expenses_Vector_Store , cargando y guardando el archivo local Expenses_policy.docx que descargó anteriormente.
   <img width="1129" height="711" alt="Captura desde 2025-10-24 11-14-30" src="https://github.com/user-attachments/assets/f6ba3aa7-2b84-48aa-aac4-0f8daaa890ef" />
   <img width="1129" height="711" alt="Captura desde 2025-10-24 11-16-13" src="https://github.com/user-attachments/assets/89d6032e-28cd-4667-9366-e5bc8755f9dc" />
   <img width="1133" height="739" alt="Captura desde 2025-10-24 11-16-47" src="https://github.com/user-attachments/assets/b88de853-d336-4255-8bb7-4600a143d3e3" />


6. En el panel Configuración, en la sección Conocimiento, compruebe que Expenses_Vector_Store aparece y se muestra como que contiene 1 archivo.<br>
   <img width="434" height="145" alt="Captura desde 2025-10-24 11-17-20" src="https://github.com/user-attachments/assets/0189f1b8-60f7-47e0-8d7b-456d970f6ebe" />

7. Debajo de la sección Conocimiento, junto a Acciones, seleccione + Agregar. A continuación, en el cuadro de diálogo Agregar acción, seleccione Intérprete de código y, a continuación, seleccione Guardar (no es necesario cargar ningún archivo para el intérprete de código).

   <img width="472" height="158" alt="Captura desde 2025-10-24 11-17-53" src="https://github.com/user-attachments/assets/e30064eb-430d-4915-b97c-c588c340d6cf" />
   <img width="1154" height="723" alt="Captura desde 2025-10-24 11-18-24" src="https://github.com/user-attachments/assets/7e2154cd-e29b-46d8-b9d7-24f1ca6a7ef8" />
   <img width="1154" height="723" alt="Captura desde 2025-10-24 11-18-35" src="https://github.com/user-attachments/assets/f9c80538-2128-4ff2-8f93-791b59eeb497" />

   Su agente utilizará el documento que cargó como fuente de conocimiento para fundamentar sus respuestas (en otras palabras, responderá preguntas basadas en el contenido de este documento). Utilizará la herramienta de intérprete de código según sea necesario para realizar acciones generando y ejecutando su propio código Python

   >**Nota**: Se recomienda dejar la configuración de temperatura lo mas cercana a 0 para evitar que el modelo alucine y que solo se utilice la información cargada<br>
   ><img width="464" height="225" alt="Captura desde 2025-10-24 11-19-53" src="https://github.com/user-attachments/assets/cd5ef447-5df2-4442-9ce7-98d977ce983f" />

   
## Pon a prueba a tu agente

Ahora que ha creado un agente, puede probarlo en el chat del playground

1. En la entrada de chat del playground, ingrese el mensaje: What's the maximum I can claim for meals? y revise la respuesta del agente, que debe basarse en la información del documento de política de gastos que agregó como conocimiento a la configuración del agente
   <img width="1083" height="511" alt="Captura desde 2025-10-24 11-21-10" src="https://github.com/user-attachments/assets/1664c902-eaa5-4b82-8755-7caa1d548937" />

2. Pruebe la siguiente indicación de seguimiento: I'd like to submit a claim for a meal y revise la respuesta. El agente debe pedirle la información requerida para presentar un reclamo.
   <img width="1083" height="511" alt="Captura desde 2025-10-24 11-22-13" src="https://github.com/user-attachments/assets/dba1dda6-2c94-454c-bdc6-92c4258e2c9d" />

3. Proporcione al agente una dirección de correo electrónico, por ejemplo fred@contoso.com. El agente debe acusar recibo de la respuesta y solicitar la información restante requerida para la reclamación de gastos (descripción y monto)
   <img width="1041" height="307" alt="Captura desde 2025-10-24 11-22-55" src="https://github.com/user-attachments/assets/abc62aca-b553-4836-b30b-bf287a231642" />

4. Envíe un mensaje que describa el reclamo y el monto, por ejemplo: Breakfast cost me $20

5. El agente debe usar el intérprete de código para preparar el archivo de texto de la reclamación de gastos y proporcionar un enlace para que pueda descargarlo.

   <img width="1043" height="418" alt="Captura desde 2025-10-24 11-23-44" src="https://github.com/user-attachments/assets/2d3de883-4c61-485a-ac00-775bf2712e47" />

6. Descargue y abra el documento de texto para ver los detalles de la reclamación de gastos.
   <img width="714" height="556" alt="Captura desde 2025-10-24 11-24-28" src="https://github.com/user-attachments/assets/5b9517b3-2011-46dd-9343-f58de3bc59a6" />

## Limpiar

Ahora que ha terminado el ejercicio, debe eliminar los recursos en la nube que ha creado para evitar el uso innecesario de recursos.

* Abra Azure Portal (https://portal.azure.com) y vea el contenido del grupo de recursos donde implementó los recursos del centro usados en este ejercicio.
* En la barra de herramientas, seleccione Eliminar grupo de recursos.
* Escriba el nombre del grupo de recursos y confirme que desea eliminarlo.




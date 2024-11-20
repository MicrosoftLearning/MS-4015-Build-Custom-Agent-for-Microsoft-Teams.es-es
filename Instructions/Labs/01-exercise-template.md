---
lab:
  title: Creación de un agente personalizado
---
<!--
Edit the metadata above to manage the list of exercises in the home page of the GitHub site that gets generated.
You can delete the module and edit index.md in the root of the repo to customize the display so that only the exercises are listed
To enable GitHub page publishing, edit the Page settings for the repo and publish from the main branch
-->

# Creación y prueba de un agente personalizado

En este ejercicio, crearás un recurso de Azure OpenAI que actúe como base para crear el agente personalizado.

Este ejercicio debería tardar en completarse **30** minutos aproximadamente. <!-- update with estimated duration -->

**Nota:** Se espera que los alumnos completen este laboratorio en sus propios entornos.

##  Tarea 1: Crear un recurso de Azure OpenAI 

Primero, necesitas...

1. Ve a **https://portal.azure.com** en tu explorador Edge.
1. Inicia sesión en Azure Portal con la credencial proporcionada en este entorno de laboratorio.
2. Haz clic en **+ Crear un recurso** en la parte superior izquierda de la pantalla.
1. En el cuadro de búsqueda escribe **azure openai** y presiona ENTRAR.
1. Un resultado denominado **Azure OpenAI** debe aparecer como una opción. En la esquina inferior izquierda de esta opción se encuentra un botón con la etiqueta **Crear**. Presiona> **Crear** > **Azure OpenAI.**
1. En la página **Crear Azure OpenAI** establece los siguientes campos: **Nota:** Dado que este laboratorio está diseñado para completarse en el propio entorno del alumno, los alumnos tendrán que usar su propio criterio a la hora de seleccionar los valores de los campos **Suscripción** , **Plan de tarifa** y **Grupo de recursos**.
   
   a. **Suscripción** usa tu propio criterio al rellenar este campo.
   
   b. **Grupo de recursos**: usa tu propio criterio al rellenar este campo.
   
   c. **Nombre**: escribe cualquier nombre que elijas.
   
   d. **Plan de tarifa**: el valor predeterminado es **Estándar S0**, pero usa tu propio criterio al rellenar este campo.
   
Seleccione **Siguiente**.

7. En la página siguiente, en la pestaña **Red**, selecciona la opción **Todas las redes, incluyendo Internet, pueden acceder a este recurso.**
Seleccione **Siguiente**.
8. En la página siguiente, en la pestaña **Etiquetas**, deja en blanco los campos Nombre y Valor.
Seleccione **Siguiente**.
9. En la página siguiente, en la pestaña **Revisar + enviar**, presiona **Crear**.
10. Se te llevará a una página donde se va a crear este nuevo recurso de Azure OpenAI. Verás las palabras **Implementación en curso**. Espera unos segundos a que este recurso termine de implementarse. Una vez implementado el recurso, haz clic en el botón **Ir al recurso**.
11. **Resultado:** ahora estarás en la página con el recurso de Azure OpenAI recién creado. Para comprobarlo, comprueba el nombre del recurso en la esquina superior izquierda de la página. Este nombre debe coincidir con el nombre que has elegido en el paso 5c anterior.


## Tarea 2: Implementación de RAG para el modelo de Azure OpenAI

Ahora vamos, ...

1. En la página del nuevo recurso de Azure OpenAI, haz clic en **Ir a Azure OpenAI Studio** en la cinta de opciones de la parte superior de la página.
2. En la nueva página titulada **Bienvenido al servicio Azure OpenAI**, haz clic en **Chat** en el menú de navegación situado a la izquierda de la pantalla.
3. En la nueva página titulada **Área de juegos de chat**, en **Configuración**, selecciona **+ Crear nueva implementación** > **A partir de modelos base**.
4. En la ventana emergente titulada **Seleccionar un modelo de finalización de chat** desplázate hacia abajo y selecciona la opción **gpt-4o** > **Confirmar**.
5. En la ventana **Implementar modelo gtp-4o**, deja su configuración predeterminada y selecciona **Implementar**.
6. En la página **Área de juegos de chat**, selecciona **Agregar los datos** ubicada cerca de la parte inferior de la pantalla > **+ Agregar un origen de datos**.
7. En la ventana **Seleccionar o agregar origen de datos**, selecciona el desplegable de **Seleccionar origen de datos** y selecciona **Cargar archivos (versión preliminar)**.
8. En la siguiente página **Origen de datos**, asegúrate de que el desplegable **Seleccionar origen de datos** está establecido en **Cargar archivos (versión preliminar)**.
   
   a. En el campo **Suscripción**, asegúrate de que está seleccionado el valor predeterminado.
   
    b. En el campo **Seleccionar recurso de Azure Blob Storage**, selecciona **Crear un nuevo recurso de Azure Blob Storage** > en la nueva ventana denominada **Crear una cuenta de almacenamiento**, en la pestaña **Aspectos básicos**, asegúrate de que los campos **Suscripción** y **Grupo de recursos** tengan los valores predeterminados; elige el único valor disponible en **Grupo de recursos**. En **Detalles de la instancia**, establece un nombre para **Nombre de la cuenta de almacenamiento**. Deja el resto de los campos como están. Seleccione **Revisar + crear**. En la pestaña **Revisar + crear**, selecciona el botón **Crear**. El recurso de Azure Blob Storage tardará un momento en implementarse.
   
   c. Vuelve a la ventana de **Chat playground**. Selecciona el botón de actualización junto al campo **Seleccionar recurso de Azure Blob storage** > selecciona el recurso que has realizado en el paso b anterior. Selecciona el botón **Activar CORS**.
   
9. En el campo **Seleccionar recurso de Azure AI Search**, selecciona **Crear un nuevo recurso de Azure AI Search**.  Asegúrate de que los campos **Suscripción** y **Grupo de recursos** están establecidos en los valores que elijas. **Nota:** Como este laboratorio está pensado para completarse en el propio entorno del alumno, éste tendrá que usar su propio criterio a la hora de seleccionar los valores para los campos **Suscripción** y, **Grupo de recursos**. Haz clic en el valor desplegable de **Grupo de recursos** para seleccionar la opción que elijas. Escribe un **Nombre de servicio**> Asegúrate de que todos los demás campos estén establecidos en sus valores predeterminados > selecciona **Revisar + crear** > **Crear**. El recurso Azure AI Search tardará un momento en implementarse.
10. Vuelve a la ventana de **Chat playground**. Selecciona el botón de actualización junto al campo **Seleccionar recurso de Azure Blob storage** > selecciona el recurso que has realizado en el paso 9 anterior.
11. Escribe un nombre para el campo **Introduce el nombre del índice** > **Siguiente**. Copia y pega este nombre en algún lugar accesible, ya que lo necesitarás en las próximas tareas.
12. En la sección **Cargar archivos**, selecciona **Examinar un archivo** > En el explorador de archivos, ve a **Documentos** > selecciona los tres archivos: **ContosoAI ChipEnhance Perks Program.docx**, **ContosoAI Insurance Plans.docx**, y **Overview of ContosoAI.docx** > **Abre** > los tres archivos deben aparecer ahora en la página **Cargar archivos** de la ventana > selecciona **Cargar archivos** > **Siguiente**.
13. En la sección **Administración de datos**, deja todo como predeterminado y selecciona **Siguiente**.
14. En **Conexión de datos** selecciona **Clave de API** > **Siguiente** > **Guardar y cerrar**.
15. En la ventana **Área de juegos del chat**, selecciona **Ver código** que se encuentra en la cinta de la parte superior izquierda de la ventana.
16. En la ventana **Código de ejemplo** selecciona el desplegable a la derecha del primer campo y selecciona **json** > cambia a la pestaña **Autenticación de claves**:
    
    a. Copia y pega los siguientes valores, ya que los necesitarás en las próximas tareas: **Punto de conexión**, **clave de API** y **Clave de recurso de Azure Search**. También puedes dejar abierta esta ventana para recopilar estos valores para las próximas tareas.

 ## Tarea 3: Crear y probar un agente personalizado en la herramienta de prueba y Teams

Ahora vamos, ...

1. Abra **Visual Studio Code**.
2. En la parte derecha de la ventana de código de Visual Studio selecciona el icono **Kit de herramientas de Teams** > selecciona **Crear una nueva aplicación** > en el desplegable selecciona **Agente de motor personalizado** (nota: según la versión del kit de herramientas de Teams, es posible que tengas que seleccionar ** Copilot personalizado**) > **Bot de chat de IA básico** > **JavaScript** > **Azure OpenAI**.
3. En el cuadro en blanco de la parte superior de la pantalla, escribe primero:

   a. **Clave de API** de la tarea anterior > **Entrar**.

   b. **Punto de conexión** de la tarea anterior > **Entrar**.

   c. En **Nombre de implementación de Azure Open AI**, escribe **gpt-4o** > **Entrar**.

   d. En **Elige la carpeta donde se ubicará la carpeta de tu sala de proyectos**, selecciona **Carpeta predeterminada**.

   e. En **Introducir nombre de aplicación** escribe cualquier nombre > **Entrar**> en la ventana emergente selecciona **Sí, confío en los autores**.

   f. En la nueva ventana de VS Code de la nueva aplicación creada a partir de los pasos anteriores, ve al icono **Kit de herramientas de Teams** en el lado izquierdo de la pantalla.

   g. En la sección **Cuentas**, haz clic en **Iniciar sesión en Microsoft 365**. Se abrirá una nueva ventana en tu navegador. Inicia sesión con las credenciales proporcionadas.

   h. Vuelve a la página de VS Code de la aplicación. Ahora deberías ver una marca de verificación verde con las palabras **Carga de aplicaciones personalizadas habilitada** en **Cuentas.

   i. En la sección **Cuentas**, haz clic en **Iniciar sesión en Azure**. Haz clic en **Aceptar** en cada ventana emergente. Se abrirá una nueva ventana en tu navegador. Inicia sesión con las credenciales proporcionadas.
   
4. Ve a **src/prompts/chat/skprompt.txt** en la ventana de VS Code de la aplicación. Elimina cualquier texto del archivo y pega lo siguiente: "La siguiente es una conversación con un asistente de IA, que es un experto en responder preguntas sobre el contexto dado. 

Las respuestas deben estar en un estilo periodístico corto con menos de 80 palabras". 

5. Ve a **config.json** archivo en mensajes/chat en la ventana de VS Code de la aplicación. Elimina el código presente actualmente entre corchetes y pega el código siguiente entre corchetes.

```json
"data_sources": [ 
    { 
        "type": "azure_search", 
        "parameters": { 
            "endpoint": "AZURE-AI-SEARCH-ENDPOINT", 
            "index_name": "YOUR-INDEX_NAME",
            "in_scope": false,
            "authentication": { 
                "type": "api_key", 
                "key": "AZURE-AI-SEARCH-KEY" 
            } 
        } 
    } 
]
```

6. En el código anterior, reemplaza lo siguiente por los valores que has guardado de la tarea anterior (nota: los valores deben estar entre comillas):

   a. **AZURE-AI-SEARCH-ENDPOINT** es el **Punto de conexión** de la tarea anterior.

   b. **index_name** es el **Index name** de la Tarea 2 paso 11 de la tarea anterior.

   c. La **clave** es la **clave de recurso de Azure Search** de la tarea anterior.

7. Ve al  **archivo src/app/app.js** y agrega la siguiente variable dentro de  **OpenAIModel** justo después de la línea azureEndpoint: config.azureOpenAIEndpoint, : 

    a. azureApiVersion: '2024-02-15-preview',
   
8. **Archivo** > **Guardar como**
    
9. Presiona **Ctrl+Shift+d** en tu teclado y aparecerá un desplegable en la parte superior izquierda que tiene un botón verde de reproducción y la palabra Depurar > Selecciona el desplegable> selecciona **Depurar en Herramienta de prueba** > Presiona **F5**.
10.El agente del motor personalizado se ejecuta en la herramienta de depuración que has elegido, que se abre en el explorador. Puedes hacer al bot cualquier pregunta relacionada con los datos RAG cargados en el paso 12 de la Tarea 2.
11. Vuelve a la página de la ventana de VS Code de la aplicación. Selecciona el desplegable del botón **Depurar** y selecciona **Depurar en Teams (Edge)** luego pulsa **F5** o el botón de reproducción verde.
13. Se abrirá una nueva ventana en tu navegador Edge. Se le solicitará que inicie sesión. Usa la información de inicio de sesión proporcionada para iniciar sesión. Una vez iniciada la sesión, cierra la ventana.
14. Repite de nuevo el paso 11. Debe haber una ventana con el título de la aplicación recién creada. Selecciona **Agregar** > **Abrir**.
15. ¡Enhorabuena! Ahora puedes hacer al agente cualquier pregunta sobre los archivos de datos RAG. 
# Comparación práctica: Blazor Server vs Blazor WebAssembly

Vamos a crear una nueva instancia de **Visual Studio Code** para observar el comportamiento de ambos modelos de Blazor.

## Procedimiento

### 1. Crear proyectos

* Abrimos Visual Studio Code y usamos la opción **“.NET: New Project”**.
* Buscamos la plantilla **Blazor** y seleccionamos **Blazor Server App**.
* Guardamos el proyecto con el nombre: `BlazorServerDemo`.

También abrimos el proyecto anterior llamado `BlazorWebAssemblyDemo`, de forma que ambos estén accesibles al mismo tiempo.

### 2. Ejecutar ambos proyectos

Ejecutamos cada uno desde su terminal respectiva con:

```bash
dotnet watch
```

Ambas aplicaciones deberían mostrarse **idénticas visualmente** en el navegador al iniciar: mismas páginas (`Home`, `Counter`, `Fetch Data`) y diseño general.

### 3. Probar comportamiento ante pérdida de conexión

Abrimos las URLs de ambos proyectos en **otro navegador** o pestaña (por ejemplo, Chrome y Firefox).

Luego, detenemos la ejecución de las terminales (cerramos la ejecución de ambos servidores en Visual Studio Code).

## Resultado

* **Blazor WebAssembly**:

  * La aplicación sigue funcionando localmente, ya que fue descargada por completo al navegador.
  * Algunas funciones que dependen del servidor (como el componente de clima) **dejarán de funcionar**, pero el resto de la interfaz sigue operativa.
  * Esto es posible porque el código C# compilado como `.dll` ya está ejecutándose dentro del navegador mediante **WebAssembly**.

* **Blazor Server**:

  * La aplicación deja de responder inmediatamente.
  * No se puede interactuar con ningún componente, ya que toda la lógica depende de la conexión en tiempo real con el servidor mediante **SignalR**.
  * Sin esa conexión, **la interfaz no puede actualizarse** ni ejecutar ninguna acción.

## Explicación técnica

* **Blazor WebAssembly**: el código está **en el cliente**, incluyendo el runtime y los componentes. Puede funcionar parcialmente sin conexión.
* **Blazor Server**: el navegador solo es una interfaz. Toda lógica está **en el servidor** y necesita conexión activa para funcionar.

Este experimento permite entender claramente la **diferencia fundamental en el comportamiento** entre ambos modelos cuando la conexión se interrumpe.

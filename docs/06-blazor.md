# 📦 Estructura de Proyecto: Blazor WebAssembly vs Blazor Server (.NET 8)

## 🔹 Blazor WebAssembly (Cliente puro)

Este modelo ejecuta toda la lógica C# en el navegador a través de **WebAssembly**. Es ideal para apps que necesitan operar parcialmente offline o ser auto-contenidas.

### Estructura típica

```bash
BlazorWasmDemo
├── App.razor
├── BlazorWasmDemo.csproj
├── Layout/
├── Pages/
├── Program.cs
├── Properties/
├── _Imports.razor
└── wwwroot/
```

### Archivos y carpetas explicados

| Elemento                         | Descripción                                                                                          |
| -------------------------------- | ---------------------------------------------------------------------------------------------------- |
| `App.razor`                      | Componente raíz. Usa `<Router>` para enrutar páginas.                                                |
| `BlazorWasmDemo.csproj`          | Define que es un proyecto `Blazor WebAssembly`. Contiene referencias y configuración de compilación. |
| `Program.cs`                     | Punto de arranque. Registra servicios y configura el renderizado WebAssembly.                        |
| `Properties/launchSettings.json` | Configura perfiles de ejecución local (solo en desarrollo).                                          |
| `_Imports.razor`                 | Centraliza los `@using`. Se aplica globalmente en los componentes `.razor`.                          |
| `Layout/`                        | Contiene layouts reutilizables (como `MainLayout.razor`) y su estilo.                                |
| `Pages/`                         | Páginas principales de la app (con `@page` para definir rutas).                                      |
| `wwwroot/`                       | Archivos estáticos. Contiene:                                                                        |
|    `index.html`                  | Punto de entrada HTML. Inicia el runtime WebAssembly y carga el componente raíz.                     |
|    `css/`, `lib/`                | Estilos (como Bootstrap) y librerías JS/CSS externas.                                                |
|    `sample-data/weather.json`    | Datos de ejemplo para simular peticiones.                                                            |

### 🔁 Flujo de ejecución (WebAssembly)

1. El navegador carga `wwwroot/index.html`.
2. Este carga `blazor.webassembly.js`.
3. Se descarga el runtime de .NET y archivos `.dll`.
4. Se ejecuta `Program.cs` y se monta `App.razor`.
5. `App.razor` enruta hacia el componente `.razor` según la URL.

## 🔹 Blazor Server

En este modelo, todo el código se ejecuta en el servidor. El navegador solo muestra la interfaz y envía eventos mediante **SignalR**.

### Estructura típica

```bash
BlazorServerDemo
├── BlazorServerDemo.csproj
├── Components/
│   ├── App.razor
│   ├── Layout/
│   ├── Pages/
│   ├── Routes.razor
│   └── _Imports.razor
├── Program.cs
├── Properties/
│   └── launchSettings.json
├── appsettings.json
├── appsettings.Development.json
└── wwwroot/
```

### Archivos y carpetas explicados

| Elemento                         | Descripción                                                                                                               |
| -------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| `BlazorServerDemo.csproj`        | Proyecto basado en `ASP.NET Core` con soporte para Razor Components.                                                      |
| `Program.cs`                     | Define el pipeline del servidor (middlewares, enrutamiento, renderizado interactivo).                                     |
| `Components/App.razor`           | Define el punto de entrada visual con `<Routes />`.                                                                       |
| `Components/Routes.razor`        | Define enrutamiento dinámico con `<Router>`. Maneja qué componente mostrar según la URL.                                  |
| `Components/Layout/`             | Layouts visuales reutilizables.                                                                                           |
| `Components/Pages/`              | Páginas principales con atributos `@page`.                                                                                |
| `Components/_Imports.razor`      | Espacios de nombres comunes para todos los componentes.                                                                   |
| `Properties/launchSettings.json` | Perfiles de ejecución locales para Visual Studio.                                                                         |
| `appsettings.json`               | Configuraciones de entorno para producción.                                                                               |
| `appsettings.Development.json`   | Configuraciones específicas para desarrollo.                                                                              |
| `wwwroot/`                       | Contenido estático (favicon, CSS, Bootstrap). No contiene `index.html`, ya que **el servidor genera HTML** dinámicamente. |

### 🔁 Flujo de ejecución (Server)

1. Se inicia la app en el servidor (`Program.cs`).
2. Se sirve una página HTML básica generada dinámicamente.
3. El navegador establece una conexión **SignalR**.
4. Toda la lógica C# se ejecuta en el servidor; el cliente solo actualiza la UI en tiempo real.
5. Si se pierde la conexión, la app deja de funcionar hasta que se restablezca.

Perfecto. A continuación, te doy la **documentación estructurada para un proyecto Blazor híbrido (WebAssembly + Server Interactivo)** usando la estructura que mostraste, compatible con **.NET 8+ y Razor Components**.

---

## 🔹 Blazor Web Assembly (híbrido WA + Server Render Mode)

Este tipo de proyecto **combina lo mejor de Blazor Server y WebAssembly**, permitiendo usar componentes interactivos que se ejecutan en el servidor, mientras se mantiene una entrega tipo SPA (Single Page Application).

### 🗂️ Estructura del proyecto

```bash
BlazorWebAssemblyDemo
├── BlazorWebAssemblyDemo.csproj
├── Components/
│   ├── App.razor
│   ├── Layout/
│   ├── Pages/
│   ├── Routes.razor
│   └── _Imports.razor
├── Program.cs
├── Properties/
│   └── launchSettings.json
├── appsettings.json
├── appsettings.Development.json
├── wwwroot/
└── BlazorWebAssemblyDemo.sln
```

### 📁 Descripción de archivos y carpetas

| Elemento                                            | Descripción                                                                                                                                                                                         |
| --------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `BlazorWebAssemblyDemo.csproj`                      | Proyecto basado en `Razor Components`. Define que usará renderizado interactivo (`.AddInteractiveServerComponents`).                                                                                |
| `Program.cs`                                        | Punto de entrada del servidor. Configura los servicios, el entorno de ejecución y define el renderizado interactivo. Aquí se usa: `app.MapRazorComponents<App>().AddInteractiveServerRenderMode();` |
| `Components/App.razor`                              | Define el punto de inicio visual de la app. Usa `<Routes />` como componente raíz para el enrutamiento.                                                                                             |
| `Components/Routes.razor`                           | Usa el componente `<Router>` para manejar la navegación y cargar componentes según la URL.                                                                                                          |
| `Components/Layout/`                                | Contiene layouts visuales compartidos. Ej: `MainLayout.razor`, que actúa como plantilla general de páginas.                                                                                         |
| `Components/Pages/`                                 | Contiene páginas de navegación (`@page`) como `Home.razor`, `Counter.razor`, etc.                                                                                                                   |
| `Components/_Imports.razor`                         | Centraliza los `@using` para evitar repetirlos en cada archivo `.razor`.                                                                                                                            |
| `Properties/launchSettings.json`                    | Configura los perfiles de ejecución de desarrollo (puertos, perfiles de Visual Studio).                                                                                                             |
| `wwwroot/`                                          | Contiene recursos estáticos como CSS (`app.css`), Bootstrap, íconos, imágenes, etc.                                                                                                                 |
| `appsettings.json` / `appsettings.Development.json` | Configuración de la app para producción y desarrollo respectivamente (conexiones, opciones, etc.).                                                                                                  |
| `BlazorWebAssemblyDemo.sln`                         | Solución de Visual Studio que agrupa todos los archivos del proyecto.                                                                                                                               |

### ⚙️ Flujo de ejecución en Blazor híbrido

1. Se ejecuta `Program.cs`, que configura servicios y habilita renderizado interactivo del lado servidor.
2. Se inyecta `App.razor`, que contiene `<Routes />` como selector de enrutamiento.
3. `Routes.razor` gestiona las rutas disponibles en la app y renderiza las páginas como `Counter`, `Home`, etc.
4. Cuando un componente necesita ser interactivo (botones, formularios, etc.), se usa `@rendermode InteractiveServer` o `@attribute [RenderModeServer]`.
5. La lógica se ejecuta **en el servidor**, pero se refleja de forma interactiva en el navegador usando **SignalR** (como en Blazor Server).
6. La carga inicial es ligera, y el contenido se entrega como HTML renderizado desde el servidor, similar a una app WebAssembly.

### 🧩 ¿Qué lo diferencia de WebAssembly puro o Server?

| Característica           | WebAssembly Puro           | Server              | **Híbrido (.NET 8+)**           |
| ------------------------ | -------------------------- | ------------------- | ------------------------------- |
| Código en navegador      | ✅ Sí                       | ❌ No                | ⚠️ Solo estática/interactiva    |
| Código en servidor       | ❌ No                       | ✅ Sí                | ✅ Sí (interactivo)              |
| `index.html`             | ✅ Sí (`wwwroot`)           | ❌ No                | ❌ No (`App.razor`)              |
| Enrutamiento             | `<Router>`                 | `<Router>`          | `<Routes>` en `App.razor`       |
| Renderizado interactivo  | En cliente (WebAssembly)   | En servidor         | En servidor (via SignalR)       |
| Flujo de carga inicial   | Todo se descarga al inicio | Solo HTML y SignalR | HTML + componentes interactivos |
| Compatibilidad navegador | Solo modernos              | Amplia              | Amplia                          |

### 📝 Ejemplo de archivo `Routes.razor`

```razor
<Router AppAssembly="typeof(Program).Assembly">
    <Found Context="routeData">
        <RouteView RouteData="routeData" DefaultLayout="typeof(Layout.MainLayout)" />
        <FocusOnNavigate RouteData="routeData" Selector="h1" />
    </Found>
</Router>
```

### 📝 Ejemplo de `Program.cs`

```csharp
var builder = WebApplication.CreateBuilder(args);

builder.Services.AddRazorComponents()
    .AddInteractiveServerComponents();

var app = builder.Build();

app.UseStaticFiles();
app.UseAntiforgery();

app.MapRazorComponents<App>()
    .AddInteractiveServerRenderMode();

app.Run();
```

## 🔸 Comparación clave

| Característica         | WebAssembly (WASM)                      | Server                          | Híbrido (.NET 8+)                           |
| ---------------------- | --------------------------------------- | ------------------------------- | ------------------------------------------- |
| Lugar de ejecución     | Navegador (cliente)                     | Servidor                        | Servidor (interactivo, con renderizado web) |
| `index.html`           | Sí (en `wwwroot`)                       | No                              | No (`App.razor` como entrada)               |
| Necesita conexión      | No (excepto para APIs externas)         | Sí                              | Sí (para interactividad)                    |
| Tamaño de descarga     | Alto (runtime + .dlls)                  | Bajo                            | Medio (HTML + señalización interactiva)     |
| Tiempo de respuesta    | Muy rápido (local)                      | Depende de la red               | Rápido, pero sujeto a conexión              |
| Acceso al servidor     | Solo vía HTTP/REST                      | Total (base de datos, archivos) | Total (como en Server)                      |
| Compatibilidad         | Solo navegadores modernos               | Ampliamente compatible          | Ampliamente compatible                      |
| Seguridad de la lógica | Expuesta (el código se descarga)        | Protegida en el servidor        | Protegida en el servidor                    |
| Offline                | Funciona offline                        | No                              | No                                          |
| Flujo de renderizado   | Todo ocurre en el cliente (WebAssembly) | En el servidor con SignalR      | HTML renderizado en servidor + SignalR      |

# Usar Blazor con Visual Studio Code

También es posible desarrollar aplicaciones Blazor usando **Visual Studio Code**, instalando previamente el SDK de .NET y utilizando la terminal.

## Requisitos

1. **Instalar el SDK de .NET**
   Descargar desde: [https://dotnet.microsoft.com/es-es/download](https://dotnet.microsoft.com/es-es/download)
   Ejecuta el instalador, acepta los permisos y sigue los pasos.

2. **Verificar la instalación**
   Abre una terminal y ejecuta:

   ```bash
   dotnet
   ```

   Si se reconoce el comando, puedes listar las plantillas disponibles con:

   ```bash
   dotnet new list
   ```

## Crear un proyecto Blazor WebAssembly

1. Abre **Visual Studio Code**.

2. Presiona `F1` y busca `“.NET: New Project”`.

3. Selecciona la plantilla:

   ```bash
   Aplicación independiente WebAssembly de Blazor   blazorwasm   [C#]   Web/Blazor/WebAssembly/PWA
   ```

4. Alternativamente, puedes crear el proyecto directamente desde la terminal:

   ```bash
   dotnet new blazorwasm -n BlazorWasmDemo
   ```

## Solución a posibles errores con NuGet

Si aparece un error relacionado con **fuentes de NuGet**, ejecuta:

```bash
dotnet nuget list source
```

Deberías ver algo como:

```bash
nuget.org [Habilitado]
    https://api.nuget.org/v3/index.json
Microsoft Visual Studio Offline Packages [Habilitado]
    C:\Program Files (x86)\Microsoft SDKs\NuGetPackages\
```

Si no aparece `nuget.org`, agrégalo con:

```bash
dotnet nuget add source https://api.nuget.org/v3/index.json -n nuget.org
```

Luego, dentro del proyecto:

```bash
dotnet restore
```

Y para iniciar el servidor de desarrollo:

```bash
dotnet watch
```

Esto abrirá automáticamente la aplicación en tu navegador predeterminado.

## Notas

* El archivo `.csproj` define la configuración del proyecto y debe estar presente en el directorio raíz.
* Puedes volver a iniciar el proyecto con:

  ```bash
  dotnet watch
  ```

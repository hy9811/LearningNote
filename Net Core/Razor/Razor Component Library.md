# Razor Component Library

Razor Component 可以透過 RCL (Razor Component Library) 建立共用元件

首先新增要使用的Razor元件庫(RCL)到 專案參考/ dll / Nuget

- 在Razor Page (.cshtml) 呼叫共用元件

```csharp
@using {APP_NAMESPACE}.Components

@*Server轉譯模式*@
<component type="typeof(ComponentName)" render-mode="ServerPrerendered"></component>

@*WebAssembly轉譯模式*@
<component type="typeof(ComponentName)" render-mode="WebAssemblyPrerendered"></component>
```

- Server轉譯模式
    - ServerPrerendered → 將元件轉譯為靜態 HTML，並包含伺服器端 Blazor 應用程式的標記
    - Server → 轉譯伺服器端 Blazor 應用程式的標記。 不包含元件的輸出。
    - Static → 將元件轉譯為靜態 HTML
- WebAssembly轉譯模式
    - WebAssembly → 轉譯 Blazor WebAssembly 應用程式的標記，以在瀏覽器中載入時用來包含互動式元件。 元件不會預先轉譯
    - WebAssemblyPrerendered → 將元件預先轉譯為靜態 HTML，並包含 Blazor WebAssembly 應用程式的標記，以供稍後在瀏覽器中載入時，讓元件成為互動式元件
- 其他特性
    - 允許多個元件標記協助程式轉譯多個 Razor 元件。
    - 應用程式啟動之後，無法動態轉譯元件。
    - 雖然頁面和檢視可以使用元件，但是反之則不成立。 元件無法使用檢視和頁面特定功能，例如部分檢視和區段。 若要從元件中的部分檢視使用邏輯，請將部分檢視邏輯分解成元件。
    - 不支援從靜態 HTML 頁面轉譯伺服器元件。

> 參考官方
[ASP.NET Core 中的元件標記協助程式 | Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/mvc/views/tag-helpers/built-in/component-tag-helper?view=aspnetcore-8.0)
> 

- 在Razor 元件中 (.razor)  呼叫共用元件

```csharp
@using {APP_NAMESPACE}.Components

@*直接呼叫使用*@
<ComponentName></ComponentName>
```
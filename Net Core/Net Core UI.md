# .Net Core UI

# 官方架構

1. **ASP.NET Core Blazor**
    - 可重複使用的元件模型。
    - 有效率的差異型元件轉譯。
    - 透過 WebAssembly 彈性地從伺服器或用戶端轉譯元件。
    - 在 C# 中建置豐富的互動式 Web UI 元件。
    - 以靜態方式從伺服器轉譯元件。
    - 漸進式增強伺服器轉譯元件，以便更順暢地流覽和表單處理，並啟用串流轉譯。
    - 在用戶端和伺服器上共用常見邏輯的程式碼。
    - 與 JavaScript 的 Interop。
    - 將元件與現有的 MVC、 Razor 頁面或 JavaScript 應用程式整合。
2. **ASP.NET Core Razor Pages**
    - 快速建置和更新 UI。 頁面的程式碼會隨著頁面保留，同時讓 UI 和商務邏輯考量區隔。
    - 可測試且可調整為大型應用程式。
    - 讓您的 ASP.NET Core 頁面以比 ASP.NET MVC 更簡單的方式組織：
        - 檢視特定的邏輯和檢視模型可以保存在自己的命名空間和目錄中。
        - 相關頁面的群組，可以保存在其自己的命名空間和目錄中。
3. **ASP.NET Core MVC**
    - 根據可調整且成熟的模型來建置大型 Web 應用程式。
    - 清楚[考量區隔](https://learn.microsoft.com/zh-tw/dotnet/standard/modern-web-apps-azure-architecture/architectural-principles#separation-of-concerns)，以取得最大的彈性。
    - Model-View-Controller 的責任區隔可確保商務模型可以發展，而不會緊密結合到低階實作詳細資料。
4. **使用前端 JavaScript 架構 ASP.NET 核心單頁應用程式 （SPA）**
    - 使用 Angular [、](https://angular.io/) [React](https://facebook.github.io/react/) 和 [Vue](https://vuejs.org/) 等 熱門 JavaScript 架構，為 ASP.NET Core 應用程式建置用戶端邏輯。 ASP.NET Core 提供 Angular、React 和 Vue 的專案範本，也可以與其他 JavaScript 架構搭配使用
    - JavaScript 執行階段環境已隨著瀏覽器提供。
    - 大型社群和成熟的生態系統。
    - 使用 Angular、React 和 Vue 等熱門 JS 架構建置 ASP.NET Core 應用程式的用戶端邏輯。
5. **選擇混合式方案：ASP.NET Core MVC 或 Razor Pages 加上 Blazor**
    - 預先轉譯會在伺服器上執行 Razor 元件，並將其轉譯成檢視或頁面，其可改善應用程式的感知負載時間。
    - 使用[元件標籤協助程式](https://learn.microsoft.com/zh-tw/aspnet/core/mvc/views/tag-helpers/built-in/component-tag-helper?view=aspnetcore-8.0)，將互動功能新增至現有的檢視或頁面。
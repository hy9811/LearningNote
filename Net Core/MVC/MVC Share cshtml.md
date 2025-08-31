# MVC Share .cshtml

使用 RCL (Razor Component Library) 選擇支援檢視與頁面(Supports pages and views)，在MyFeature(可更名)底下，會產生Page1範本(可更換)。

在MVC 專案參考引用 RCL，引用後直接啟動，瀏覽到MyFeature/Page1，可直接導向到RCL 底下的cshtml 頁面

- 在RCL 的cshtml中，可能會無法使用Partial標籤協助器，但可以使用 `Html.PartialAsync`

> https://github.com/dotnet/aspnetcore/issues/51364
> 
- 共用RCL 的cshtml，目前還未找到有效的Partial引用方式，可能嘗試的範例如下
都使用重新設定RazorPage需要Assembly的資料夾

> [Using Razor Class Library (RCL) to generate a common UI for all your dotnet web projects | by Emmanuel D. | The Startup | Medium](https://medium.com/swlh/using-razor-class-library-rcl-to-generate-a-common-ui-for-all-your-dotnet-web-projects-be970d4a82a0)
[c# - How to render a partial view located in an application part (DLL) into main asp.net core 2.2 project - Stack Overflow](https://stackoverflow.com/questions/65365778/how-to-render-a-partial-view-located-in-an-application-part-dll-into-main-asp)
[c# - Consuming a Razor Class Library with Partial Views in .Net Framework Project - Stack Overflow](https://stackoverflow.com/questions/62106832/consuming-a-razor-class-library-with-partial-views-in-net-framework-project)
> 

→ (已解決)

1. 在Nuget 安裝 `Microsoft.ASpNetCore.Mvc.Razor.RuntimeCompilation`
2. 在安裝完成後，把RCL 專案/Publish 路徑指向Razor動態執行編譯

```csharp
// 在Program.cs中，加入此段設定
 builder.Services.AddRazorPages().AddRazorRuntimeCompilation(options => 
 {
     var libraryPath = Path.GetFullPath("專案資料夾");
     options.FileProviders.Add(new PhysicalFileProvider(libraryPath));
 });
```

1. 在自己專案中的cshtml中，使用以下方式加入

```csharp
@await Html.PartialAsync("/Areas/MyFeature/Pages/TestPartial.cshtml")
```

> 參考作法
[razor - Render RazorPage to string from a RazorClassLibrary fails at rendering partial - Stack Overflow](https://stackoverflow.com/questions/53687215/render-razorpage-to-string-from-a-razorclasslibrary-fails-at-rendering-partial)
> 

依照以上作法，可以建立引用Razor Page元件，仍然為靜態頁面

效果

![Untitled](Net%20Core/MVC/MVC%20Share%20cshtml/Untitled.png)

![Untitled](Net%20Core/MVC/MVC%20Share%20cshtml/Untitled%201.png)
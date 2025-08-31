# Razor Component

Razor Component可以使用Code behind，直接在.razor相同的資料夾底下新增相同名稱的類別，其附檔名為.razor.cs，即完成Code behind

> 生命週期
[魔術技巧 - 掌握元件生命週期 - iT 邦幫忙::一起幫忙解決難題，拯救 IT 人的一天 (ithome.com.tw)](https://ithelp.ithome.com.tw/articles/10245981)
[ASP.NET Core Razor 元件生命週期 | Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/blazor/components/lifecycle?view=aspnetcore-3.1)
> 

Razor Component可以引用外部的專案/服務

- 參考引用

參考引用後，需要在razor程式中使用以下方法引用

```csharp
@using {ReferenceName}.Test.Code // 引用命名空間
```

- 服務注入

參考引用後，需要在Promgram.cs中注入

```csharp
using {ReferenceName}.Test.Code
// ...
builder.Service.AddSingleton(InjectService); //註冊服務生命週期為啟動時建立單例
// 若有針對共通性服務建立Interface，則可以使用
// builder.Service.AddSingleton(IinjectService, InjectService);
// ...
```

服務注入後，可以在razor程式中注入

```csharp
@using {ReferenceName}.Test.Code
@inject InjectService injectService

@Code {
	private Model model;
	private override async Task OnInitialized() // 在Razor元件建立時
	{
		model = await injectService.GetAsync(); // 範例非同步呼叫API
	}
}
```

依照以上作法，可以在Razor元件中使用其他已經完成的API/服務

> 參考 [Blazor 使用 RCL](Blazor%20使用%20RCL.md) 提供的範本程式
>
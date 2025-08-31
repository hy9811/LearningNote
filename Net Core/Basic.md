# Basic

---

## .Net Core Lifecycle

## Option

- 慣例是使用Option建立binding類別(Class)，透過DI注入Configuration服務，並指定組態檔(appsetting.json)的特定區段(Section)為binding對應的Option項。
- 通常不直接注入IConfiguration到Controller

> 參考官方文件
[ASP.NET Core 中的選項模式 | Microsoft Learn](https://learn.microsoft.com/zh-tw/aspnet/core/fundamentals/configuration/options?view=aspnetcore-8.0)
>
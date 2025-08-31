# MVC

## 基本概念

以MVC為基礎的應用程式包含

- Models</br>
代表應用程式資料的類別，以及使用驗證邏輯來強制執行該資料的商務規則，
模型物件是實作應用程式資料欄邏輯的應用程式部分。
通常，模型物件會擷取和儲存資料庫中的模型狀態
- Views</br>
應用程式用來動態產生 HTML 回應的範本檔案，
顯示應用程式使用者介面的元件 UI ，通常是從模型資料建立。
- Controllers</br>
處理傳入瀏覽器要求的類別、擷取模型資料，然後指定檢視範本以傳回瀏覽器的回應。
可以處理使用者互動、使用模型並且在最後選擇可以轉譯要顯示 UI 的檢視。

## Visual Studio MVC 專案

MVC 專案分兩類型，[ASP.NET](http://asp.net/) MVC 與 [ASP.NET](http://asp.net/) Core
新增專案 [ASP.NET](http://asp.net/) Web Application(.NET Framework)

> 差異介紹 </br>
https://learn.microsoft.com/zh-tw/dotnet/architecture/porting-existing-aspnet-apps/architectural-differences
> 

專案內包含 Model，View，Controller 的資料夾

## [ASP.NET](http://asp.net/) MVC 執行

1. 請求會先通過 URLRoutingModule 物件。
2. UrlRoutingModule物件會剖析該要求，選取符合目前要求的第一個路由物件。
3. 取得與Route物件相關聯的IRouteHandler物件。
4. IRouteHandler實例會建立IHttpHandler物件，並將IHttpCoNtext物件傳遞給它。
5. MVC 的 IHttpHandler 實例是 MvcHandler 物件，會選取最終會處理要求的控制器。

模組和處理常式是 [ASP.NET](http://asp.net/) MVC 架構的進入點，進入後的執行動作為

1. 選取 MVC Web 應用程式中適當的控制器。
2. 取得特定控制器執行個體。
3. 呼叫控制器的 Execute 方法。

下列列出 MVC Web 專案的執行階段：

1. 接收應用程式的第一個要求</br>
在 Global.asax 檔案中， Route 物件會新增至 RouteTable 物件。
2. 執行路由</br>
UrlRoutingModule模組會使用RouteTable集合中第一個相符的Route物件來建立RouteData物件，然後用它來建立RequestCoNtext (IHttpCoNtext) 物件。
3. 建立 MVC 要求處理常式</br>
MvcRouteHandler物件會建立MvcHandler類別的實例，並將RequestCoNtext實例傳遞給它。
4. 建立控制器</br>
MvcHandler物件會使用RequestCoNtext實例來識別IControllerFactory物件， (通常是DefaultControllerFactory類別的實例，) 用來建立控制器實例。
5. 執行控制器 - MvcHandler 實例會呼叫控制器的 Execute 方法。
6. 叫用動作
大部分的控制器都繼承自 Controller 基類。 如果是這樣做的控制器，與控制器相關聯的 ControllerActionInvoker 物件會決定要呼叫的控制器類別的動作方法，然後呼叫該方法。
7. 執行結果
典型的動作方法可能會接收使用者輸入、準備適當的回應資料，然後傳回結果類型來執行結果。 可執行檔內建結果類型包括： ViewResult (，它會轉譯檢視，而且是最常使用的結果類型) 、 RedirectToRouteResult、 RedirectResult、 ContentResult、 JsonResult和 EmptyResult。

## URL

URL在ASP.NET中對應的是控制器動作。
以下是在 App_Start 資料夾中的 RouteConfig.cs的預設路由表

```csharp
public static void RegisterRoutes(RouteCollection routes)
{
    routes.IgnoreRoute("{resource}.axd/{*pathInfo}");

    routes.MapRoute(
        name: "Default",                                                                    // Route name
        url: "{controller}/{action}/{id}",                                                  // URL with parameters
        defaults: new { controller = "Home", action = "Index", id = UrlParameter.Optional } // Parameter defaults
    );
}

```

預設路徑如程式中，defaults 的格式為 /\[Controller]/\[ActionName]/\[Parameters]

- 控制器：Home
- 動作：Index
- 識別碼：空字串
假設URL為 /Product/Details/3，則
- 控制器：Product
- 動作：Details
- 識別碼：3

接續說明控制器 Controller

## Controller

以下為預設的 HomeController.cs。

```csharp
public class HomeController : Controller
{
    public ActionResult Index()
    {
        return View();
    }

    public ActionResult About()
    {
        ViewBag.Message = "Your application description page.";

        return View();
    }

    public ActionResult Contact()
    {
        ViewBag.Message = "Your contact page.";

        return View();
    }
}

```

HomeController包含 Index()， About()， Contact()方法
當 URL ( /Home/Index ) 請求傳入後，會對應到控制器 HomeController.Index()

在 Contorller中新增以下程式可以測試路由邏輯

```csharp
public string Welcome(string name, int numTimes = 1) {
     return HttpUtility.HtmlEncode("Hello " + name + ", NumTimes is: " + numTimes);
}

```

在瀏覽器網址列輸入 URL ( /Home/Welcome?name=Scott&numtimes=4 )
在上述的 URL 中，</br>
<b>查詢字串</b>為　name和numTimes</br>
<b>分隔符號</b>為　?</br>
<b>分隔查詢區段符號</b>為　&

### 新增 Controller

首先在 App_Start\RouteConfig.cs 中新增路由表

```c
routes.MapRoute(
           name: "Hello",
           url: "{controller}/{action}/{name}/{id}"
       );

```

在 Visual Studio 的專案中的 Controller 資料夾右鍵新增 HelloController 控制器，
會自動產生 Controller 程式碼。

```c
public class HelloController : Controller
{
    // GET: Hello
    public ActionResult Index()
    {
        return View();
    }
}

```

新增路由邏輯

```c
public string Welcome(string name, int id = 1)
{
    return HttpUtility.HtmlEncode("Hello " + name + ", ID: " + id);
}

```

<font color=red>此處有異常</font></br>
開啟瀏覽器將參數資訊從 URL 傳遞至控制器，例如
URL ( /Hello/Welcome/Scott/3 )

輸出結果

```
Hello Scott, ID: 3

```

## View

View 是以 cshtml 建立的， cshtml 可以在 html 中加入 C# 語法，稱為 Razor 語法

> Razor</br>
https://learn.microsoft.com/zh-tw/aspnet/core/razor-pages/?view=aspnetcore-7.0&tabs=visual-studio
> 

### 新增View

在 Views\Hello 資料夾右鍵，選擇 [新增]，
然後按一下 [具有版面配置 (Razor) 的 MVC 5 檢視頁面]</br>
輸入名稱 Index，選擇版面配置 _Layout.cshtml

在新建的 Index.cshtml 開頭程式片段會指定版面配置

```c
@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}

```

註解方式如下，註解後，將不會套用任何版面，
若要改變版面，可以修改 Layout 的屬性值，
或是設定為 null 也同樣不會套用版面。

```c
@*@{
    Layout = "~/Views/Shared/_Layout.cshtml";
}*@

```

在 Index.cshtml 中新增標記

```c
@{
    ViewBag.Title = "Index";
}

<h2>Index</h2>

<p>Hello from our View Template!</p>

```

上述的新增動作中 _Layout.cshtml 是版面配置頁面，與其他頁面共用，
版面範本可以在某個位置指定網站的 HTML 容器配置，
並套用到網站中的多個頁面。</br>
在 _Layout.cshtml 中 `@RenderBody()` 是預留位置，
所有檢視特定頁面會顯示在版面配置頁面中。</br>
例如 URL ( Views\Home\About.cshtml )

在以下 Razor 標記中，會對應到 _Layout.cshtml 的 `<title>@ViewBag.Title - Home App</title>`

```c
@{
    ViewBag.Title = "Index";
}

```

修改 Index.cshtml 和 _Layout.cshtml，實際結果修改瀏覽器標題與內文標頭

```c
<!--Index.cshtml-->
@{
    ViewBag.Title = "Movie List";
}

<h2>My Movie List</h2>

<p>Hello from our View Template!</p>

```

```c
<!-- _Layout.cshtml-->
<title>@ViewBag.Title - Movie App</title>

```

接續說明 Model

## Model

### 新增模型類別

在 Models 資料夾新增類別 Movie，
每個物件實例對應至資料庫資料表內的資料列，
每個物件屬性都會對應至資料表中的資料行。</br>
範例如下

```c
public class Movie
{
    public int ID { get; set; }
    public string Title { get; set; }
    public DateTime ReleaseDate { get; set; }
    public string Genre { get; set; }
    public decimal Price { get; set; }
}

public class MovieDBContext : DbContext
{
    public DbSet<Movie> Movies { get; set; }
}

```

### 新增連線

在專案根目錄下的 Web.Config (不是Views資料夾中的Web.config檔案。)
尋找`<connectionStrings>`標記，新增以下內容

```xml
<add name="MovieDBContext"
     connectionString="Data Source=(LocalDb)\\MSSQLLocalDB;Initial Catalog=aspnet-MvcMovie;Integrated Security=SSPI;AttachDBFilename=|DataDirectory|\\Movies.mdf"
     providerName="System.Data.SqlClient"
/>

```

實際上不需要新增 MovieDBContext 連接字串。
如果您未指定連接字串，Entity Framework 將會在使用者目錄中建立 LocalDB 資料庫，
並在此案例 MvcMovie.Models.MovieDBContext 中建立DbContext類別的完整名稱 ()

### 控制器連接模型範例

類別模型新增後，建置專案，
在 Controllers 資料夾下新增控制器，
選擇<b>具有檢視、使用Entity Framework 的 MVC 5 控制器</b>

選擇模型類型為 Movie，資料內容類別為 MovieDBContext，輸入控制器名稱為 MovieController，

會自動建立以下檔案和資料夾：

- Controllers 資料夾中的 MoviesController.cs 檔案。
- Views\Movies 資料夾。
- 在新的 Views\Movies 資料夾中建立 .cshtml、Delete.cshtml、Details.cshtml、Edit.cshtml 和 Index.cshtml。

Visual Studio 會自動建立 CRUD (建立、讀取、更新和刪除) 動作方法和檢視
在 _Layout.cshtml 的 `<div class="container">` 內新增

```
 @Html.ActionLink("MVC Movie", "Index", "Movies", Movies, new { @class = "navbar-brand" })

```

執行專案後，點選 MVC Movie 即進入 Movie 控制器

### 控制器存取模型資料

在 MovieController.Index()</br>
控制器的要求 Movies 會傳回資料表中的所有 Movies 專案，然後將結果傳遞至 Index 檢視。

在 MovieController.Details()</br>
若 URL( /Movie/Details/1 )，控制器設定為 MovieController、動作設定為 Details，
並將 ID 設為 1。如果找到 Movie ID，模型實例 Movie 會傳遞至 Details 檢視。

```c
public ActionResult Details(int? id)
{
    if (id == null)
    {
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Movie movie = db.Movies.Find(id);
    if (movie == null)
    {
        return HttpNotFound();
    }
    return View(movie);
}

```

### 強型別模型和 @model 關鍵字

在Views\Movies\Details.cshtml 首行，
@model 指示詞使用強型別的 Movie 物件，存取控制器傳遞至檢視的電影。

```
@model mvc.Models.Movie

```

在Views\Movies\Index.cshtml 首行，

```
@model IEnumerable<mvc.Models.Movie>

```

@model 指示詞使用強型別的 Movie 物件，存取控制器傳遞至檢視的電影清單。
例如以下foreach迴圈中，以強型別 Movie 物件執行。

```c
@foreach (var item in Model) {
    <tr>
        <td>
            @Html.DisplayFor(modelItem => item.Title)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.ReleaseDate)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Genre)
        </td>
        <td>
            @Html.DisplayFor(modelItem => item.Price)
        </td>
         <th>
            @Html.DisplayFor(modelItem => item.Rating)
        </th>
        <td>
            @Html.ActionLink("Edit", "Edit", new { id=item.ID }) |
            @Html.ActionLink("Details", "Details", new { id=item.ID })  |
            @Html.ActionLink("Delete", "Delete", new { id=item.ID })
        </td>
    </tr>
}

```

Model 因為是強型別 (做為 `IEnumerable\\<Movie\\>` 物件)，
所以迴圈中的每個 item 都為 Movie 物件。

### 檢查編輯方法與編輯檢視

在 Movie.cs 的欄位中修改

```c
[Display(Name = "Release Date")]
[DataType(DataType.Date)]
[DisplayFormat(DataFormatString = "{0:d}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }

```

此步驟是針對日期格式化輸出，使瀏覽器只顯示日期，不會顯示時間。

在 Views\Movie\Index.cshtml 中，Edit 按鈕透過`Html`物件的`ActionLink`連結到Edit.cshtml

```c
@Html.ActionLink("Edit", "Edit", new { id=item.ID })

```

`ActionLink`在此範例中，有三個參數

1. 顯示文字
2. 呼叫的動作
3. 匿名物件

假設點選 ID 為 4 的 movie，則 URL 為 (/Movies/Edit/4)，
因為 App_Start\RouteConfig.cs 中建立的預設路由會採用 URL 模式，
也可以使用查詢字串傳遞動作方法參數，則 URL 為 (/Movies/Edit?ID=4)

檢視 MovieController.cs 的 `Edit()`方法

```c
// GET: Movies/Edit/5
public ActionResult Edit(int? id)
{
    if (id == null)
    {
        return new HttpStatusCodeResult(HttpStatusCode.BadRequest);
    }
    Movie movie = db.Movies.Find(id);
    if (movie == null)
    {
        return HttpNotFound();
    }
    return View(movie);
}

// POST: Movies/Edit/5
// 若要免於大量指派 (overposting) 攻擊，請啟用您要繫結的特定屬性，
// 如需詳細資料，請參閱 <https://go.microsoft.com/fwlink/?LinkId=317598。>
[HttpPost]
[ValidateAntiForgeryToken]
public ActionResult Edit([Bind(Include = "ID,Title,ReleaseDate,Genre,Price")] Movie movie)
{
    if (ModelState.IsValid)
    {
        db.Entry(movie).State = EntityState.Modified;
        db.SaveChanges();
        return RedirectToAction("Index");
    }
    return View(movie);
}

```

第一個 Edit 動作方法預設套用 `HttpGet`，
使用Entity Framework `Find` 方法依據 ID 尋找 movie，
將結果回傳至 Edit.cshtml 顯示，如果尋找不到，則會回傳 `HttpNotFound`。

第二個 Edit 動作方法則是套用 `HttpPost` 屬性，
並使用 `Bind` 設定只能使用指定屬性，
以及`ValidateAntiForgeryToken` 驗證檢視中呼叫所產生的 `@Html.AntiForgeryToken( )` XSRF權杖，
防止跨網站偽造要求。

> HTTP 方法</br>
https://www.w3schools.com/tags/ref_httpmethods.asp</p>
Bind 方法</br>
https://learn.microsoft.com/zh-tw/dotnet/api/system.web.mvc.bindattribute?view=aspnet-mvc-5.2&redirectedfrom=MSDN</p>
跨網站偽造要求</br>
https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/implementing-basic-crud-functionality-with-the-entity-framework-in-asp-net-mvc-application#overpost</p>
MVC 的 XSRF/CSRF 預防</br>
https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
> 

Scaffolding 系統建立 Edit 檢視時，它會檢查 Movie 類別，
並建立程式碼為類別的每個屬性轉譯 `<label>` 和 `<input>` 元素。

```c
<form action="/movies/Edit/4" method="post">
   <input name="__RequestVerificationToken"
        type="hidden" value="hidden XSRF token" />
        <fieldset class="form-horizontal">

```

元素 `<input>` 位於 HTML `<form>` 元素中，其 action 屬性設定為張貼至 /Movies/Edit URL。
按下 [ Save ] 按鈕時，表單資料將會張貼到伺服器。

動作的步驟為

1. Edit 的 post 方法動作為採用`form`的值，建立 Movie 物件做為參數傳遞。
2. ModelState.IsValid 會驗證表單中提交的資料可用來修改 (編輯或更新物件) Movie。
3. 如果資料有效，會儲存至 Movies (MovieDBContext 實例的 db 集合)。
4. 新的電影資料會藉由呼叫 SaveChanges 的 MovieDBContext 方法來儲存至資料庫。
5. 儲存資料之後，程式碼將使用者重新導向至 MoviesController 類別的 Index 動作方法，
此方法會顯示包括剛剛所進行變更的電影集合。

一旦用戶端驗證判斷欄位的值無效，就會由Edit.cshtml檢視範本中的`Html.ValidationMessageFor`協助程式負責顯示適當的錯誤訊息。

### 新增搜尋

在 MovieController.cs 中 `Index` 修改

```c
public ActionResult Index(string searchString)
{
    var movies = from m in db.Movies
                 select m;

    if (!String.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    return View(movies);
}

```

新增的程式判斷查詢字串是否為空值，並使用 LINQ 作為查詢方法，
查詢電影是否查詢字串，並將結果回傳。

在此處的 LINQ `Contain()`對應到 SQL 的 `LIKE` ，並在資料庫上執行，
movies 會直到被呼叫或是使用立即執行的方法才會動作。

使用 URL (/Movies/Index?searchString=ghost)，即可查詢。

若要將查詢字串轉移至 Route Data 則修改如下

```c
public ActionResult Index(string id)
{
    string searchString = id;
    ...
}

```

修改後即可使用 URL (/Movies/Index/ghost) 進查詢。

接續在 Views\Movies\Index.cshtml 的段落 `@Html.ActionLink("Create New", "Create")` 後新增

```c
@using (Html.BeginForm()){
    <p> Title: @Html.TextBox("SearchString") <br/>
    <input type="submit" value="Filter"/></p>
}

```

Html.BeginForm 會在開頭建立 <form> 標記，
在使用者按下 [ filter ] 按鈕來提交表單時，讓表單 post 給自己。

此時注意到 URL 並無直接對應到查詢字串，因為 `POST` 與 `GET` 方法的 URL 相同，
查詢字串會以表單形式傳送至伺服器。

若要在 URL 中顯示查詢字串，則修改上段程式的 `@using (Html.BeginForm())`

```c
@using (Html.BeginForm("Index","Movies",FormMethod.Get))

```

接續在 MovieController.cs `Index()` 中修改

```c
public ActionResult Index(string movieGenre, string searchString)
{
    var GenreLst = new List<string>();

    var GenreQry = from d in db.Movies
                   orderby d.Genre
                   select d.Genre;

    GenreLst.AddRange(GenreQry.Distinct());
    ViewBag.movieGenre = new SelectList(GenreLst);

    var movies = from m in db.Movies
                 select m;

    if (!String.IsNullOrEmpty(searchString))
    {
        movies = movies.Where(s => s.Title.Contains(searchString));
    }

    if (!string.IsNullOrEmpty(movieGenre))
    {
        movies = movies.Where(x => x.Genre == movieGenre);
    }

    return View(movies);
}

```

上述程式新增內容分類型查詢，建立 `GenreLst` 作為查詢結果儲存，
`GenreQry` 在資料表中排序並選擇所有內容類型，
合併重複項之後使用`AddRange()`傳入`GenreLst`中，
存入的List內容會以 `SelectList` 建立，作為下拉式清單方塊的存取資料。

在 Views\Movies\Index.cshtml 新增下拉式清單方塊

```
@using (Html.BeginForm("Index", "Movies", FormMethod.Get))
{
<p>
    Genre: @Html.DropDownList("movieGenre", "All")
    Title: @Html.TextBox("SearchString")
    <input type="submit" value="Filter" />
</p>
}

```

`Html.DropDownList` 會建立下拉式清單方塊，
並尋找 `IEnumerable<SelectListItem>` 的 `ViewBag` 索引鍵 `DropDownList`。

參數 「All」 提供選項標籤，並且為空值，由於 MovieController 只會篩選 if 字串不是 null 或空白，
因此提交的 movieGenre 空白值會顯示所有內容類型。

接續針對模型新增欄位

### 新增欄位

> 設定模型變更Code First 移轉</br>
https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/getting-started/introduction/adding-a-new-field#setting-up-code-first-migrations-for-model-changes
> 

參考上面的做法轉移資料模型後，新增 Rating 屬性到 Movie 模型中
新增下列屬性至 Movie 類別

```c
public string Rating { get; set; }

```

在 MovieController.cs 更新 Bind 的 Create 和 Edit 屬性

```c
[Bind(Include = "ID,Title,ReleaseDate,Genre,Price,Rating")]

```

在 \Views\Movies\Index.cshtml 在 Price 後新增

```c
<table class="table">
...
    <th>
        @Html.DisplayNameFor(model => model.Rating)
    </th>
...

@foreach (var item in Model) {
    <td>
        @Html.DisplayFor(modelItem => item.Rating)
    </td>
</table>

```

在 \Views\Movies\Create.cshtml 在 Price 後新增

```c
<div class="form-group">
    @Html.LabelFor(model => model.Rating, new { @class = "control-label col-md-2" })
    <div class="col-md-10">
        @Html.EditorFor(model => model.Rating, new { htmlAttributes = new { @class = "form-control" } })
        @Html.ValidationMessageFor(model => model.Rating, "", new { @class = "text-danger" })
    </div>
</div>

```

在 Configuration.cs 的每個欄位新增

```c
new Movie
{
    Title = "When Harry Met Sally",
    ...
    Rating = "PG",
    ...
}

```

新增後 在套件管理器主控台輸入</br>
`add-migration Rating`

命令 `add-migration` 會告訴移轉架構使用目前的電影 DB 架構檢查目前的電影模型，
並建立必要的程式碼，以將資料庫移轉至新的模型。
Rating 是用來命名移轉檔案名稱。

輸入後會產生 *DataStamp*_Rating.cs輸入指令</br>
`update-database`

### 新增驗證

在 Movie 類別修改，利用內建的 `Required`、`StringLength`、`RegularExpression`和 `Range` 驗證屬性

```c
public class Movie
{
    public int ID { get; set; }

    [StringLength(60, MinimumLength = 3)]
    public string Title { get; set; }

    [Display(Name = "Release Date")]
    [DataType(DataType.Date)]
    [DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
    public DateTime ReleaseDate { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z]*$")]
    [Required]
    [StringLength(30)]
    public string Genre { get; set; }

    [Range(1, 100)]
    [DataType(DataType.Currency)]
    public decimal Price { get; set; }

    [RegularExpression(@"^[A-Z]+[a-zA-Z]*$")]
    [StringLength(5)]
    public string Rating { get; set; }
}

```

更新後在套件管理器主控台使用指令更新資料庫

```
add-migration DataAnnotations
update-database

```

驗證 UI 運作方式
在 \Views\Movies\Create.cshtml
會在`ModelState.IsValid` 判斷輸入是否驗證錯誤

除了內建的驗證方法，
`System.ComponentModel.DataAnnotations`也提供了格式屬性。
例如已經套用 DataType 列舉值的發行日期和價格欄位，如下

```c
[DataType(DataType.Date)]
public DateTime ReleaseDate { get; set; }

[DataType(DataType.Currency)]
public decimal Price { get; set; }

```

> Required
[https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)
StringLength
[https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)
RegularExpression
[https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)
Range
[https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)
DataType
[https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)
> 

### 檢查 Details 和 Delete 方法

在 Index 點選 Delete 按鈕時，會呼叫 GET 方法顯示電影詳細資訊，
在電影詳細資訊點選 Delete 按鈕，才會使用 POST 執行刪除。

```c
// GET: /Movies/Delete/5
public ActionResult Delete(int? id)

//
// POST: /Movies/Delete/5
[HttpPost, ActionName("Delete")]
public ActionResult DeleteConfirmed(int id)

```

### 參考資料

強型別

> https://learn.microsoft.com/zh-tw/archive/blogs/rickandy/dynamic-v-strongly-typed-viewshttps://learn.microsoft.com/zh-tw/aspnet/mvc/overview/views/dynamic-v-strongly-typed-views
> 

Scaffolding

> https://learn.microsoft.com/zh-tw/aspnet/visual-studio/overview/2013/aspnet-scaffolding-overview
> 

基本概念

> https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
> 

Razor

> https://learn.microsoft.com/zh-tw/aspnet/web-pages/overview/getting-started/introducing-razor-syntax-c
> 

EF

> https://learn.microsoft.com/zh-tw/aspnet/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application
> 

[ASP.NET](http://asp.net/)

> https://learn.microsoft.com/zh-tw/dotnet/architecture/porting-existing-aspnet-apps/architectural-differences
> 

[MVC Share .cshtml](MVC%20Share%20cshtml.md)

[View Component](View%20Component.md)
# Blazor 使用 RCL

Blazor 是使用Razor component 組合建立畫面，所以在RCL中建立的元件也可以被引用至Blazor

> 參考 [Razor Component Library](Razor%20Component%20Library.md)
> 

建立後啟動，會由Blazor 轉譯元件

以下範本

建立可執行API的元件

```csharp
@using RazorClassLibrary.Components
@using CSH.ChatTask.Models

<button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target="#testModal" @onclick="Open">
  開啟彈跳視窗 顯示Test清單
</button>
@if(showModal)
{
<div class="modal fade @modalClass" id="testModal" aria-labelledby="testModal" style="display:@modalDisplay; overflow-y: auto;">
    <div class="modal-dialog">
        <div class="modal-content">
            <div class="modal-header">
                <h5 class="modal-title" id="testModalLabel">Modal title</h5>
            </div>
            <div class="modal-body">
                <div class="Container">
                    <h5>Test清單</h5>
                    <div class="container">
                        <table class="table">
                            <thead>
                                <tr>
                                    <th>PromptId</th>
                                    <th>PromptName</th>
                                </tr>
                                </thead>
                            <tbody>
                                @foreach (ChatTaskPrompt prompt in prompts)
                                {
                                    <tr>
                                        <td>@prompt.PromptId</td>
                                        <td>@prompt.PromptName</td>
                                    </tr>
                                }
                            </tbody>
                        </table>
                    </div>
                </div>
            </div>
            <div class="modal-footer">
                <button type="button" class="btn btn-secondary" data-bs-dismiss="modal" @onclick="Close">Close</button>
            </div>
        </div>
    </div>   
</div>
}

@code
{
    [Parameter]
    public bool showModal {get; set;} = false;

    [Parameter]
    public string modalClass{get; set;} = "";

    [Parameter]
    public string modalDisplay {get; set;} = "none;";

    private List<string> titles { get; set; } = new List<string>();
    public async Task LoadTestList()
    {        
        prompts = await Task.Run(() => LoadTestAPI.ToList()); // 此處可以修改成其他API
    }

    public async Task Open()
    {
        showModal = true;
        modalClass = "show";
        modalDisplay = "block;";
        await LoadTestList();
        StateHasChanged();
    }
    
    public void Close()
    {
        showModal = false;
        modalClass = "";
        modalDisplay = "none";
        StateHasChanged();
    }
}
```

效果

![Untitled](Net%20Core/Blazor/Blazor%20使用%20RCL/Untitled.png)

![Untitled](Net%20Core/Blazor/Blazor%20使用%20RCL/Untitled%201.png)
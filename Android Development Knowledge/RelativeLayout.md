# RelativeLayout

### 1. 核心概念

- 以相對彼此或父容器的關係來擺放子 View（舊世代相對布局）。
- 多數情境已被 ConstraintLayout 取代，但閱讀舊專案仍常見。

### 2. 常見屬性

- `layout_alignParentStart|End|Top|Bottom`
- `layout_toStartOf|toEndOf|above|below`、`layout_centerInParent` 等

### 2.1 屬性詳解與用法

- 父容器對齊類（不依賴其他子 View）
    - `layout_alignParentStart|End|Top|Bottom="true"`
        - 使此 View 貼齊父容器對應邊緣。
    - `layout_centerInParent="true"`
        - 同時水平與垂直置中，等同於 `centerHorizontal + centerVertical`。
    - `layout_centerHorizontal|layout_centerVertical="true"`
        - 分別在水平或垂直方向置中。
- 相對兄弟 View 類（需先有目標 View 的 id）
    - `layout_toStartOf|toEndOf="@id/target"`
        - 將此 View 放到 target 的起點或終點一側。
    - `layout_above|below="@id/target"`
        - 在垂直方向上，置於 target 的上方或下方。
    - `layout_alignStart|alignEnd|alignTop|alignBottom="@id/target"`
        - 與 target 的對應邊緣對齊。
    - `layout_alignBaseline="@id/target"`
        - 與 target 的文字基線對齊，適合同字體大小的文字元件。
- 尺寸與間距
    - `layout_width|layout_height="wrap_content|match_parent|具體dp"`
    - `layout_margin*` 系列：`layout_marginStart|End|Top|Bottom`，支援 RTL；亦可用 `layout_margin` 設定四邊相同值。
    - `padding*` 是 View 自身內距，與 `margin*`（外距）不同。
- 方向性與相容性
    - 建議使用 `Start/End` 而非 `Left/Right` 以支援 RTL。
    - 舊屬性如 `layout_toLeftOf|toRightOf` 在新專案不建議使用。

### 2.2 基本範例

```xml
<RelativeLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:padding="16dp">

    <TextView
        android:id="@+id/title"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Title"
        android:textSize="18sp"
        android:textStyle="bold"
        android:layout_alignParentStart="true"
        android:layout_alignParentTop="true"/>

    <TextView
        android:id="@+id/subtitle"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Subtitle"
        android:layout_below="@id/title"
        android:layout_alignStart="@id/title"
        android:layout_marginTop="8dp"/>

    <ImageView
        android:id="@+id/avatar"
        android:layout_width="40dp"
        android:layout_height="40dp"
        android:src="@drawable/ic_avatar"
        android:contentDescription="avatar"
        android:layout_alignParentEnd="true"
        android:layout_centerVertical="true"/>

    <Button
        android:id="@+id/action"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="Action"
        android:layout_toStartOf="@id/avatar"
        android:layout_alignBaseline="@id/avatar"
        android:layout_marginEnd="12dp"/>
</RelativeLayout>
```

說明：

- `title` 固定在父容器左上角。
- `subtitle` 在 `title` 之下，並與其起點對齊。
- `avatar` 貼齊父容器右側並垂直置中。
- `action` 夾在 `avatar` 左側，並與其基線對齊。

### 2.3 規則優先與常見陷阱

- 規則衝突時，可能出現不可預期結果：例如同時 `alignParentStart=true` 與 `toEndOf` 指向他 View。
    - 建議「父對齊」與「相對兄弟」選擇其一主軸來組合。
- 缺少目標 id 會使相對屬性無效：務必確保 `@+id/...` 已存在。
- 巢狀 RelativeLayout 容易造成量測多次與效能下降，能合併則合併，或改用 ConstraintLayout。
- RTL 支援：統一使用 `Start/End` 與 `layout_marginStart|End`，避免硬編 Left/Right。

### 2.4 何時用 RelativeLayout、更佳替代

- 少量、簡單的相對關係，或維護舊碼時仍可用。
- 多條相依關係、需要鏈式約束、比例或指引線時，改用 ConstraintLayout 更清晰與高效。
- 維護舊專案或極少量、簡單相對關係。
- 新開發通常建議改用 ConstraintLayout。

### 4. 注意

- 易造成巢狀與複製約束，維護成本高。
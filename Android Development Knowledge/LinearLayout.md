# LinearLayout

### 1. 核心概念

- LinearLayout 以「單一方向」排列子 View，方向由 `android:orientation` 決定。
    - `vertical`：沿垂直方向自上而下排列。
    - `horizontal`：沿水平方向自左而右排列。
- 子 View 彼此不重疊，按加入順序依次排列。

### 2. 子視圖位置與尺寸

- `layout_width` 與 `layout_height` 一般配合 `wrap_content` 或 `match_parent` 使用。
- `layout_gravity` 可控制子 View 在剩餘空間中的對齊位置（例如 `center`、`end`）。
- 透過 `layout_weight` 進行剩餘空間分配：
    - 權重值越大，分到的剩餘空間越多。
    - 權重常與某一維度設為 `0dp` 搭配，以明確告知「由權重決定尺寸」。

### 3. 用途與常見場景

- 直向或橫向的簡單列表或表單區塊。
- 小型工具列、設定項、按鈕列。
- 配合 `weight` 做等分或比例分配的排版。

### 4. 效能特性

- 比 `RelativeLayout` 舊版或複雜約束更輕量，但深層巢狀多層 LinearLayout 仍會增加量測成本。
- 若巢狀過深，建議改用 `ConstraintLayout` 以扁平化層級。

📌 簡單理解：

LinearLayout 就像是一條「單行道」，所有子 View 要嘛直直排，要嘛橫著排，必要時用權重來分配剩餘空間。

### 5. 常見屬性與小技巧

- `android:orientation="vertical|horizontal"`：決定主軸方向。
- `android:gravity` 與子 View 的 `layout_gravity`：前者影響容器內整體對齊，後者影響單個子 View 在剩餘空間內的對齊。
- `android:divider` 與 `showDividers`：可快速加入分隔線。
- `layout_weight`：做等分或比例分配，避免硬編 dp。

### 6. 權重規則與注意

- 權重參與分配的是「剩餘空間」。
- 常見作法：在分配維度上把子 View 該維度設 `0dp`，完全由權重決定尺寸。
- 權重計算可能導致多次量測，注意效能。

### 7. 觸控與可點擊

- 與 FrameLayout 類似，最上層可點擊 View 會先攔截事件；但 LinearLayout 通常不疊層，較少衝突。
- 若只用來排版，避免把 LinearLayout 設為 `clickable`，以免阻擋子 View 事件。

### 8. 無障礙（Accessibility）

- 線性排列有助於螢幕閱讀順序自然流動。
- 重要元素提供 `contentDescription`，避免裝飾性 View 參與焦點。

### 9. 何時不建議使用 LinearLayout

- 排版需要對齊依賴或相對關係多時，使用 `ConstraintLayout` 更佳。
- 需要網格或表格式排版時，考慮 `TableLayout` 或 RecyclerView 的 Grid。

### 10. 範例片段

- 垂直列表基本型：

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:orientation="vertical"
    android:padding="16dp">

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Title" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:text="Subtitle" />
</LinearLayout>
```

- 權重等分：

```xml
<LinearLayout
    android:layout_width="match_parent"
    android:layout_height="48dp"
    android:orientation="horizontal">

    <Button
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:text="Left" />

    <Button
        android:layout_width="0dp"
        android:layout_height="match_parent"
        android:layout_weight="1"
        android:text="Right" />
</LinearLayout>
```

### 11. 效能最佳化建議

- 減少巢狀層級，能合併則合併。
- 使用權重時留意重複量測成本；靜態等分可直接用固定寬度或 `ConstraintLayout`。
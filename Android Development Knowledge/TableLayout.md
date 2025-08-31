# TableLayout

### 1. 核心概念

- TableLayout 以列（TableRow）與欄的形式呈現，適合表格式資訊。
- 每個 `TableRow` 代表一列；列內的子 View 預設依加入順序形成多欄。
- 支援欄位合併（`layout_span`）等特性，但不具備自動欄寬一致化的複雜規則（較簡單）。

### 2. 子視圖位置與尺寸

- TableLayout 本身不顯示欄線；可用背景或自訂 Drawable 繪製邊線。
- `TableRow` 高度通常由內容決定；欄寬受各儲存格內容影響。
- 透過 `layout_column` 指定儲存格落在第幾欄；用 `layout_span` 進行跨欄。

### 3. 用途與常見場景

- 靜態設定頁、規格對照表、鍵值對資訊、簡易行列表。
- 小型費用明細、成績單段落、對齊需求明確的區塊。

### 4. 效能特性

- 結構簡單，適合小型、固定資料量的表格。
- 大型或動態資料建議使用 RecyclerView + GridLayoutManager 以獲得更好的效能與可回收性。

📌 簡單理解：

TableLayout 像是「迷你表格」，快速排出幾行幾列的對齊資訊，但不適合承載大量動態清單。

### 5. 常見屬性與小技巧

- `shrinkColumns`、`stretchColumns`：指定哪些欄可縮放或延展，以適應版面。
- `collapseColumns`：可隱藏特定欄。
- `TableRow` 內的子 View 可搭配 `layout_span` 做跨欄標題或總結列。
- 以 `weightsum` 並不適用於 TableRow，若需比例，考慮內層 LinearLayout 或使用 ConstraintLayout。

### 6. 欄寬與對齊

- 欄寬非全域一致，受各列內容影響；如果需要嚴格等寬，考慮用 ConstraintLayout 或自訂測量。
- 對齊可在儲存格的子 View 以 `gravity` 或 `layout_gravity` 控制。

### 7. 觸控與可點擊

- 列點擊常用於選取或展開細節；可把 `TableRow` 設置 `clickable` 與前景 ripple。
- 儲存格中的互動元件要留意焦點競爭，確保預期的點擊目標能獲得事件。

### 8. 無障礙（Accessibility）

- 清楚的表頭與欄位語意有助於螢幕閱讀器理解；以描述性文字或在首列加標題強化語意。
- 避免用純圖示表達關鍵資訊，或提供替代文字。

### 9. 何時不建議使用 TableLayout

- 需要大量、可捲動、可回收的清單或表格時，用 RecyclerView 及其 Grid/Span 機制。
- 需要複雜條件顯示或響應式版面時，考慮 ConstraintLayout 或 Compose。

### 10. 範例片段

- 簡易兩欄鍵值對表格：

```xml
<TableLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:stretchColumns="1">

    <TableRow>
        <TextView
            android:text="Name" />
        <TextView
            android:text="Alice" />
    </TableRow>

    <TableRow>
        <TextView
            android:text="Age" />
        <TextView
            android:text="24" />
    </TableRow>
</TableLayout>
```

- 跨欄標題：

```xml
<TableLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TableRow>
        <TextView
            android:text="Monthly Report"
            android:textStyle="bold"
            android:layout_span="2"
            android:gravity="center"
            android:padding="8dp" />
    </TableRow>

    <TableRow>
        <TextView android:text="Income" />
        <TextView android:text="$10,000" />
    </TableRow>
</TableLayout>
```

### 11. 效能最佳化建議

- 僅用於少量靜態內容；大量資料使用 RecyclerView。
- 边線使用單一背景 Drawable 避免過多 shape 疊加。
- 若需要等寬欄位，考慮改用 `ConstraintLayout` 以比例或 Guideline 實現。
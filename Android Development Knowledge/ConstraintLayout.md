# ConstraintLayout

### 1. 核心概念

- ConstraintLayout 以「約束」描述子 View 與父容器或其他子 View 的關係，透過少量層級實現複雜排版。
- 支援直向與橫向同時約束、偏移、比例、鏈結（Chains）等能力。

### 2. 子視圖位置與尺寸

- 以 `app:layout_constraint*` 屬性建立邊到邊的連結，如：
    - `app:layout_constraintStart_toStartOf="parent"`
    - `app:layout_constraintEnd_toEndOf="parent"`
- 尺寸模式：
    - `wrap_content`：由內容決定。
    - `0dp`（match constraints）：由約束與比例決定。
    - `match_parent` 不建議常用，通常以 0dp + 約束取代。
- 偏移與位移：`bias` 可微調對齊位置（0 到 1）。

### 3. 用途與常見場景

- 取代多層 LinearLayout 或 RelativeLayout 的複雜巢狀。
- 做響應式比例布局、等分、基準線對齊、疊層控制等。
- 與 MotionLayout 配合做轉場與互動動畫。

### 4. 效能特性

- 以一層容器完成多數排版，減少量測與繪製成本。
- 約束解析需要計算，但通常仍優於深度巢狀布局。

📌 簡單理解：

ConstraintLayout 就像是「畫布 + 尺規」，用連線與比例把每個元件放在想要的位置。

### 5. 常見屬性與小技巧

- 鏈結（Chains）：在同一軸向以 0dp 並列，透過 `layout_constraintHorizontal_chainStyle` 設定 `packed`、`spread`、`spread_inside`。
- 比例：`app:layout_constraintDimensionRatio="W,H"` 或 `"16:9"`，須配合一側為 0dp。
- 指導線（Guideline）與屏障（Barrier）：建立可重用的對齊基準與動態邊界。
- 群組（Group）與層（Layer）：批次控制可見性與轉換。
- Baseline 對齊：文字基準對齊更精準。

### 6. 疊層與 Z 軸

- 以 `android:elevation` 與 `translationZ` 控制層級。
- 可直接重疊子 View（不需像 FrameLayout 依順序），依賴 Z 軸與順序解決覆蓋需求。

### 7. 觸控與可點擊

- 元件可重疊時，確保互動目標位於可點擊層級，避免透明 View 擋住事件。
- 大型可點擊區域可用 Layer 或 MotionLayout 一併處理手勢與動效。

### 8. 無障礙（Accessibility）

- 約束不影響焦點順序，仍以視圖階層與加入順序為主；必要時調整 `focusOrder` 或程式控制。
- 清楚的 `contentDescription` 與對比度，避免裝飾性元素搶焦。

### 9. 何時不建議使用 ConstraintLayout

- 極簡單的一維排列可用 LinearLayout 更直觀。
- 純表格型或清單型資料應使用 TableLayout 或 RecyclerView。

### 10. 範例片段

- 基本四邊對齊與置中：

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <TextView
        android:id="@+id/title"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintHorizontal_bias="0.5"
        android:text="Title" />

    <Button
        android:id="@+id/action"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_constraintTop_toBottomOf="@id/title"
        app:layout_constraintEnd_toEndOf="parent"
        android:text="Go" />
</androidx.constraintlayout.widget.ConstraintLayout>
```

- 鏈結與比例：

```xml
<androidx.constraintlayout.widget.ConstraintLayout
    android:layout_width="match_parent"
    android:layout_height="120dp">

    <ImageView
        android:id="@+id/cover"
        android:layout_width="0dp"
        android:layout_height="0dp"
        app:layout_constraintDimensionRatio="16:9"
        app:layout_constraintStart_toStartOf="parent"
        app:layout_constraintEnd_toStartOf="@id/info"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent" />

    <LinearLayout
        android:id="@+id/info"
        android:orientation="vertical"
        android:layout_width="0dp"
        android:layout_height="wrap_content"
        app:layout_constraintEnd_toEndOf="parent"
        app:layout_constraintTop_toTopOf="parent"
        app:layout_constraintBottom_toBottomOf="parent"
        app:layout_constraintStart_toEndOf="@id/cover"
        app:layout_constraintHorizontal_chainStyle="packed">

        <!-- 文字等子 View -->

    </LinearLayout>
</androidx.constraintlayout.widget.ConstraintLayout>
```

### 11. 效能最佳化建議

- 以單層 ConstraintLayout 取代多層巢狀容器。
- 對需要等分的區塊，以 0dp + 鏈結或比例取代 LinearLayout 權重。
- 減少過度複雜的約束網，必要時切分為數個區域或使用包含的子布局。
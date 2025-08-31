# FrameLayout

### 1. 單一子視圖為主

- 設計上 FrameLayout **預設只適合放置一個子元件**，這個子元件會填滿整個 FrameLayout。
- 當放入多個子元件時，**後添加的子元件會疊加在前一個上面**（類似堆疊效果）。

### 2. 子視圖位置

- 子元件會依照 **layout_gravity** 屬性決定在 FrameLayout 中的位置。
    - 例如：`android:layout_gravity="center"` 可以讓子元件置中。
- 沒有設定 `layout_gravity` 時，預設是放在左上角。

### 3. 用途與常見場景

- **作為簡單的容器**：常見於想只放置一個子元件時。
- **覆蓋與堆疊效果**：
    - 放多個元件時，後放的會覆蓋前面的，可以用來做圖片與文字的覆疊。
    - 例如：一張圖片上方疊一個播放按鈕。
- **Fragment 的容器**：由於它簡單，常被用來作為 `FragmentTransaction.replace()` 的容器。
- **背景控制**：可以放置一個背景 View，再在上面加其他內容。

### 4. 效能特性

- 相較於 `LinearLayout` 或 `RelativeLayout`，`FrameLayout` 較輕量，因為它沒有複雜的測量與排版邏輯。
- 適合用在需要單一元件或疊層的情況，避免過度使用多層巢狀的複雜布局。

📌 **簡單理解**：

`FrameLayout` 就像是一個「畫布」，你可以在上面放一個主要元件，或是多個元件讓它們一層層疊在一起。

### 5. 常見屬性與小技巧

- `android:foreground` 與 `android:foregroundGravity`：可在 FrameLayout 上放置前景（例如 ripple 或遮罩）。
- `android:clipToPadding` 與 `android:clipChildren`：控制子 View 是否能畫出邊界外，做陰影或出血效果時很有用。
- `android:foregroundTint`：搭配前景圖示著色，實現統一色系。
- `android:padding` 與子 View 的 `layout_margin`：疊層時常用 padding 打底、由子 View 以 margin 做細部位置微調。
- `android:layout_gravity` 與 `android:gravity` 差異：
    - `layout_gravity` 決定「子 View 在父容器中的位置」。
    - `gravity`（設在父容器）決定「父容器如何擺放沒有 layout_gravity 的子 View 的內容對齊」。

### 6. 疊層與 Z 軸行為

- 相同父容器中，後加入的子 View 會在視覺上位於前面。
- 可搭配 `elevation` 與 `translationZ` 控制陰影與點擊回饋層級。
- 需要互動的前景元素（例如播放按鈕），務必確保它的區塊不被其他透明但可點擊的 View 擋住。

### 7. 觸控事件與可點擊區塊

- 疊層時，最上層且可點擊的 View 會先攔截事件。
- 若上層 View 僅作裝飾，請避免設為 `clickable` 或 `focusable`，以免阻擋底層互動。
- 複雜互動建議將可點擊區塊做明確大小，或使用 `ViewCompat.setOnApplyWindowInsetsListener` 等協助處理手勢衝突。

### 8. 可存取性與無障礙（Accessibility）

- 疊層容易造成焦點順序混亂。
- 為重要元素加上 `contentDescription` 或無障礙標籤，並調整焦點順序，避免裝飾性 View 參與焦點。
- 按鈕等互動元素需有足夠觸控範圍與對比度。

### 9. 何時不建議使用 FrameLayout

- 需要相對定位、依賴對齊關係或多條件約束時，考慮 `ConstraintLayout`。
- 需要直向或橫向線性排列時，考慮 `LinearLayout`，或直接用 `ConstraintLayout` 以減少巢狀層級。

### 10. 範例片段

- 單一容器承載 Fragment：

```xml
<FrameLayout
    android:id="@+id/container"
    android:layout_width="match_parent"
    android:layout_height="match_parent" />
```

- 圖片上疊播放鍵與漸層遮罩：

```xml
<FrameLayout
    android:layout_width="200dp"
    android:layout_height="120dp">

    <ImageView
        android:id="@+id/cover"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:scaleType="centerCrop"
        android:src="@drawable/sample" />

    <View
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@drawable/gradient_scrim" />

    <ImageButton
        android:id="@+id/btnPlay"
        android:layout_width="48dp"
        android:layout_height="48dp"
        android:layout_gravity="center"
        android:background="?attr/selectableItemBackgroundBorderless"
        android:src="@drawable/ic_play" />
</FrameLayout>
```

- 使用前景 ripple 效果（避免額外包一層）：

```xml
<FrameLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content"
    android:clickable="true"
    android:foreground="?attr/selectableItemBackground"
    android:padding="16dp">

    <!-- 子 View 放這裡 -->

</FrameLayout>
```

### 11. 效能最佳化建議

- 盡量維持扁平結構，疊層只放必要元素。
- 能用 `foreground`、`background` 解決的，就不要多包一層 View。
- 圖片請使用正確尺寸與快取機制，避免在小螢幕放入超大圖造成繪製成本上升。
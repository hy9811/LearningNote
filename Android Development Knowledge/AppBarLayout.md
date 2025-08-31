# AppBarLayout

## 概念

AppBarLayout 是一個垂直方向的 LinearLayout，通常置於 CoordinatorLayout 內作為應用欄容器，搭配 CollapsingToolbarLayout 與 Toolbar 可實現捲動折疊效果。

## 何時使用

- 需要 ToolBar 與標題區在捲動時收合或固定
- 希望遵循 Material Design 上方應用欄模式

## 常見結構與旗標

```xml
<[com.google.android](http://com.google.android).material.appbar.AppBarLayout
    android:layout_width="match_parent"
    android:layout_height="wrap_content">

    <[com.google.android](http://com.google.android).material.appbar.CollapsingToolbarLayout
        android:layout_width="match_parent"
        android:layout_height="240dp"
        app:layout_scrollFlags="scroll|exitUntilCollapsed|snap">

        <ImageView
            android:layout_width="match_parent"
            android:layout_height="match_parent"
            android:scaleType="centerCrop"
            app:layout_collapseMode="parallax"
            app:layout_collapseParallaxMultiplier="0.7"/>

        <[com.google.android](http://com.google.android).material.appbar.MaterialToolbar
            android:id="@+id/toolbar"
            android:layout_width="match_parent"
            android:layout_height="?attr/actionBarSize"
            app:layout_collapseMode="pin"/>

    </[com.google.android](http://com.google.android).material.appbar.CollapsingToolbarLayout>
</[com.google.android](http://com.google.android).material.appbar.AppBarLayout>
```

### layout_scrollFlags 常用值

- scroll：允許跟隨捲動
- enterAlways：下滑立即顯示 AppBar
- exitUntilCollapsed：上滑直到最小高度才停止
- snap：停在就近狀態，手感更自然
- enterAlwaysCollapsed：與 enterAlways 合作，先顯示最小高度

### 內容區連動

在可捲動內容上設定：

```xml
app:layout_behavior="@string/appbar_scrolling_view_behavior"
```

## Compose 對照：連動 TopAppBar

```kotlin
val scrollBehavior = TopAppBarDefaults.exitUntilCollapsedScrollBehavior()
Scaffold(
  topBar = {
    LargeTopAppBar(
      title = { Text("Title") },
      scrollBehavior = scrollBehavior
    )
  }
) { inner ->
  LazyColumn(
    modifier = Modifier
      .padding(inner)
      .nestedScroll(scrollBehavior.nestedScrollConnection)
  ) { /* items */ }
}
```

## 常見坑與排查

- 未設置 appbar_scrolling_view_behavior 內容不跟隨
- CollapsingToolbarLayout 未給定最小高度或標題模式，收合後布局重疊
- snap 與 enterAlwaysTogether 組合需測試手感

## 最佳實務

- 善用 CollapsingToolbarLayout 的 titleEnabled 與 scrim 相關屬性
- 測試不同密度與字體縮放下的折疊效果
- 與 WindowInsets 配合，避免狀態列內容被遮擋
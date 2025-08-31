# CoordinatorLayout

## 概念

CoordinatorLayout 是一個強大的容器，協調其子 View（特別是與 Behavior 搭配的子 View）之間的互動，例如 AppBar 收合、FAB 跟隨捲動、BottomAppBar 隱藏顯示等。

## 何時使用

- 需要 AppBarLayout 與內容區塊產生連動效果
- 需要透過 Behavior 實作捲動時的特殊互動
- 需要支援邊到邊與沉浸式狀態列處理

## 依賴與命名空間

```
implementation "androidx.coordinatorlayout:coordinatorlayout:1.2.0"
implementation "[com.google.android](http://com.google.android).material:material:<latest>"
```

## 常見結構（XML）

```xml
<androidx.coordinatorlayout.widget.CoordinatorLayout
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <[com.google.android](http://com.google.android).material.appbar.AppBarLayout
        android:layout_width="match_parent"
        android:layout_height="wrap_content">

        <[com.google.android](http://com.google.android).material.appbar.CollapsingToolbarLayout
            android:layout_width="match_parent"
            android:layout_height="240dp"
            app:layout_scrollFlags="scroll|exitUntilCollapsed">

            <ImageView
                android:layout_width="match_parent"
                android:layout_height="match_parent"
                android:scaleType="centerCrop"
                app:layout_collapseMode="parallax"/>

            <[com.google.android](http://com.google.android).material.appbar.MaterialToolbar
                android:id="@+id/toolbar"
                android:layout_width="match_parent"
                android:layout_height="?attr/actionBarSize"
                app:layout_collapseMode="pin"/>
        </[com.google.android](http://com.google.android).material.appbar.CollapsingToolbarLayout>
    </[com.google.android](http://com.google.android).material.appbar.AppBarLayout>

    <androidx.recyclerview.widget.RecyclerView
        android:id="@+id/list"
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        app:layout_behavior="@string/appbar_scrolling_view_behavior"/>

    <[com.google.android](http://com.google.android).material.floatingactionbutton.FloatingActionButton
        android:id="@+id/fab"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        app:layout_anchor="@id/list"
        app:layout_anchorGravity="bottom|end"
        app:layout_behavior="[com.google.android](http://com.google.android).material.behavior.HideBottomViewOnScrollBehavior"/>
</androidx.coordinatorlayout.widget.CoordinatorLayout>
```

## 常用 Behavior

- appbar_scrolling_view_behavior：讓內容區塊與 AppBarLayout 捲動連動
- HideBottomViewOnScrollBehavior：捲動時隱藏底部元件（如 BottomAppBar、FAB）
- BottomSheetBehavior：實作可拖曳的底部面板
- SwipeDismissBehavior：實作滑動關閉元件
- 自訂 Behavior：繼承 CoordinatorLayout.Behavior<T>，攔截 NestedScroll 事件實作互動

## Kotlin 片段（Toolbar 綁定）

```kotlin
val toolbar = findViewById<MaterialToolbar>([R.id](http://R.id).toolbar)
setSupportActionBar(toolbar)
supportActionBar?.setDisplayHomeAsUpEnabled(true)
```

## WindowInsets（沉浸式）

```kotlin
WindowCompat.setDecorFitsSystemWindows(window, false)
ViewCompat.setOnApplyWindowInsetsListener(findViewById([R.id](http://R.id).root)) { v, insets ->
  val sys = insets.getInsets(WindowInsetsCompat.Type.systemBars())
  v.updatePadding(top = [sys.top](http://sys.top), bottom = sys.bottom)
  insets
}
```

## 常見坑與排查

- 可捲動內容未設定 app:layout_behavior，AppBar 不會收合
- 父層不是 CoordinatorLayout，Behavior 不生效
- 子 View 使用 match_parent 導致覆蓋或滑動衝突
- CollapsingToolbarLayout 高度與 scrim 未妥善配置導致閃爍

## 最佳實務

- 僅在需要協調互動的畫面使用，以減少複雜度
- 優先使用現成 Behavior，必要時再自訂
- 與 WindowInsets 協作，處理狀態列與導航列邊距

## Compose 對照

- Scaffold(topBar = ..., floatingActionButton = ...)
- TopAppBar scrollBehavior + Modifier.nestedScroll 建立連動
- 若需複雜互動，使用 NestedScrollConnection 自訂行為
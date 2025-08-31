# View Group Family Tree

![Untitled diagram _ Mermaid Chart-2025-08-29-015859.png](Untitled_diagram___Mermaid_Chart-2025-08-29-015859.png)

```jsx
---
config:
  theme: dark
  look: classic
---
classDiagram

%% 基底
class View
class ViewGroup
View <|-- ViewGroup

%% 核心 ViewGroup
ViewGroup <|-- LinearLayout
ViewGroup <|-- RelativeLayout
ViewGroup <|-- FrameLayout
ViewGroup <|-- ConstraintLayout
ViewGroup <|-- GridLayout
LinearLayout <|-- TableLayout

%% 滾動容器
FrameLayout <|-- ScrollView
FrameLayout <|-- HorizontalScrollView
FrameLayout <|-- NestedScrollView

%% 清單/大量項目
ViewGroup <|-- AdapterView
AdapterView <|-- AbsListView
AbsListView <|-- ListView
AbsListView <|-- GridView
ViewGroup <|-- RecyclerView

%% 特殊/動畫切換容器
FrameLayout <|-- ViewAnimator
ViewAnimator <|-- ViewFlipper
ViewAnimator <|-- ViewSwitcher
ViewSwitcher <|-- TextSwitcher
ViewSwitcher <|-- ImageSwitcher

%% Navigation / Material / Coordinator 架構
ViewGroup <|-- DrawerLayout
ViewGroup <|-- CoordinatorLayout
LinearLayout <|-- AppBarLayout
ViewGroup <|-- Toolbar %% (android.widget / androidx.appcompat.widget)
FrameLayout <|-- NavigationView
FrameLayout <|-- BottomNavigationView
HorizontalScrollView <|-- TabLayout
ViewGroup <|-- ViewPager
ViewGroup <|-- ViewPager2

%% 系統外層容器（概念）
FrameLayout <|-- DecorView

%% 已廢棄
ViewGroup <|-- AbsoluteLayout

```
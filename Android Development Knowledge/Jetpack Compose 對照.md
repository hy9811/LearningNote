# Jetpack Compose 對照

### 1. 對照表

- Box ≈ FrameLayout（疊層與前景元素）
- Row ≈ LinearLayout horizontal
- Column ≈ LinearLayout vertical
- ConstraintLayout（Compose 版）≈ 約束式佈局

### 2. 差異重點

- Compose 為宣告式 UI，狀態驅動與可組合性，無傳統 View 階層限制。
- 以 Modifier 取代 XML 屬性，如 padding、weight、align。
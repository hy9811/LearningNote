# GridLayout

### 1. 核心概念

- 以列與欄構成的格狀佈局，適合小型、靜態的網格。

### 2. 與 RecyclerView Grid 的取捨

- 小型固定內容：GridLayout 簡單即可。
- 大量或動態清單：RecyclerView + GridLayoutManager 更佳（回收與效能）。

### 3. 常見屬性

- `rowCount`、`columnCount`、`layout_row`、`layout_column`、`layout_rowSpan`、`layout_columnSpan`。
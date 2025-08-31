# Layout 常見誤用與排查

### 常見誤用

- LinearLayout 濫用權重導致多次量測與卡頓。
- FrameLayout 透明可點擊 View 阻擋底層事件。
- ConstraintLayout 過度複雜約束，難以維護。
- TableLayout 欄寬不一致造成視覺對齊落差。

### 排查建議

- 使用 Layout Inspector 觀察層級與量測次數。
- 優先扁平化、減少巢狀，能 0dp+比例就別用 weight。
- 檢查可點擊與可聚焦屬性，確保事件路徑符合預期。
# OpenSpec 工作指引（給 AI 助手）

本資料夾是本專案的規格來源。修改行為或內容前，先讀 `project.md` 與相關的 `specs/`。

## 工作流程

1. **提案**：新增需求、改變行為或大範圍修改論文時，在 `changes/<change-id>/` 建立：
   - `proposal.md`：要改什麼、為什麼改、影響範圍。
   - `tasks.md`：可勾選的實作清單。
   - `specs/<capability>/spec.md`：以 `## ADDED`、`## MODIFIED`、`## REMOVED` 描述規格差異。
2. **實作**：依 `tasks.md` 逐項完成，完成一項勾選一項。
3. **歸檔**：變更完成並驗證後，把差異合併進 `specs/<capability>/spec.md`，再把該 change 移到 `changes/archive/<date>-<change-id>/`。

小的錯字或格式修正不需要提案，直接修改即可。

## 規格格式

- 每個能力一個資料夾：`specs/<capability>/spec.md`。
- 需求用 `### Requirement: <名稱>`，並以 SHALL 或 MUST 描述。
- 每個需求至少一個情境，使用 `#### Scenario: <名稱>`，內容為 **WHEN** 與 **THEN**。

## 禁止事項

- 不在論文或規格中寫入未經來源支持的外部事實或統計數字。
- 不宣稱尚未完成的實驗有結果。
- 不在提交的檔案中留下紅色佔位（`\textcolor{red}`）或 TODO。
- 不在公開 repo 中放入憑證、token 或 cookie。

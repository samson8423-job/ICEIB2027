# 論文稿件規格

## Requirements

### Requirement: 外部事實必須有來源
論文中的每一個外部統計數字與文獻結果 MUST 附上引用，且引用條目 MUST 存在於 `references.bib`。

#### Scenario: 引用缺失
- **WHEN** 正文出現未附引用的外部統計數字
- **THEN** 該句 MUST 被補上引用或改寫為有來源支持的內容

### Requirement: 不宣稱未完成的實驗結果
論文 MUST NOT 把尚未執行的實驗描述為已完成。實驗章節只能描述規劃、指標與目標值。

#### Scenario: 實驗尚未執行
- **WHEN** 實驗章節描述效能數字
- **THEN** 該數字 MUST 標示為目標值，並說明尚未量測

### Requirement: 提交前無佔位與 TODO
提交版本的 LaTeX 原始檔 MUST NOT 含有 `\textcolor{red}` 佔位或 TODO 註記，且編譯 MUST 無錯誤與未定義的引用。

#### Scenario: 編譯檢查
- **WHEN** 同步到 Overleaf 並執行 `compile_project`
- **THEN** `summary.builtCleanly` MUST 為 `true`，且 `undefinedCitations` 與 `undefinedReferences` MUST 為空

### Requirement: 頁數與格式
論文 MUST 使用 IEEEtran conference 格式，頁數依 ICEIB 2027 的規定（網站說明為 4–8 頁）。

#### Scenario: 頁數檢查
- **WHEN** 編譯完成
- **THEN** `pageCount` MUST 落在主辦單位規定的範圍內

## 相關文件

- 現行稿件的章節與來源對照見 `myRef/A2Z-SOP-reference.md`。

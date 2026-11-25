# DIC4 工作紀錄：AI Fitness Coach 期末專題與 ICEIB 2027 論文

本資料夾記錄從「連上 GitHub、整理專題資料」到「用 MCP 把 IEEE 論文同步到 Overleaf 並編譯」的完整過程，包括中途遇到的問題與解法，方便之後重現或接手。

## 目錄結構

| 路徑 | 內容 |
|---|---|
| `ICEIB2027-IEEE/` | IEEE 論文（LaTeX 原始檔），即 ICEIB 2027 投稿草稿 |
| `myRef/` | 參考資料：A2Z SOP 範例、提案解析 PDF、資訊圖表、`A2Z-SOP-reference.md` 對照表 |
| `.mcp.json` | 專案層級的 MCP 設定（Overleaf MCP server） |
| `.vscode/mcp.json` | VS Code 格式的 MCP 設定（Claude Code 不會讀取，見下方問題 5） |

## 一、整體流程

建議的順序是**先建立 MCP 連線，再請 agent 修改論文**。這樣每次修改都能立刻同步到 Overleaf 並編譯驗證。

1. **建立 MCP 連線**：在根目錄放好 `.mcp.json`，重新啟動 Claude Code，執行 `/mcp` 確認 `overleaf` 已連線，再用 `auth_status` 確認已登入（見第三節）。
2. **確認 Overleaf 專案與本機資料夾對應**：用 `list_projects` 找到專案 ID，用 `get_project_tree` 確認檔案結構與根文件是 `main.tex`，用 `plan_sync` 比對本機 `ICEIB2027-IEEE/` 與專案的差異（只比對、不修改）。
3. **整理參考資料**：閱讀 `myRef/` 的提案解析、A2Z SOP 範例與資訊圖表，產生 `myRef/A2Z-SOP-reference.md`。
4. **請 agent 修改論文**：在本機 `ICEIB2027-IEEE/` 修改 `main.tex` 與各章節，填入 AI Fitness Coach 內容。
5. **查證與修正**：用網路搜尋核對統計數字與引用，修正超出來源的敘述，補上 bib 條目。
6. **同步與編譯**：每次修改後重新 `plan_sync`，確認計畫後用 `sync_directory`（`mode: additive`、`writeMode: tracked`）同步，再用 `compile_project` 編譯，確認 `builtCleanly` 為 `true`。若有 BibTeX 錯誤，用 `get_compile_log` 查看後修正，再重複第 6 步。
7. **加入架構圖**：依 `design.md` 的 Mermaid 圖，用 TikZ 重畫成 8 模組的架構圖，並納入同步與編譯。
8. **推送 GitHub**：確認本機結果後，將 `ICEIB2027-IEEE/`、`.mcp.json` 與 `myRef/` 推到 `samson8423-job/ICEIB2027`。

> **注意**：本專案的實際順序與上面不同。前期的章節是在 MCP 尚未連上時直接在本機編輯，之後才用 `plan_sync` 一次同步到 Overleaf。之後的專案請依照上面的順序進行，可以省去大量事後同步與除錯的時間。

## 二、論文 `ICEIB2027-IEEE/` 的結構與修改方式

```
ICEIB2027-IEEE/
├── main.tex              # 標題、作者、套件、章節的 \input 進入點
├── references.bib        # 參考文獻（BibTeX）
├── sections/
│   ├── 00_abstract.tex
│   ├── 01_introduction.tex
│   ├── 02_related_work.tex       # 含 Table I（文獻摘要表）
│   ├── 03_proposed_scheme.tex    # 含公式、Algorithm 1、架構圖（TikZ）
│   ├── 04_experiments.tex        # 實驗規劃（尚未有實測結果）
│   └── 05_conclusion.tex
├── tables/comparison.tex # 各系統能力比較表（依來源回報，非實測）
└── figures/README.md
```

### 主要修改原則

- **只寫有來源或本專案設計的內容**。外部事實（統計數字、文獻結果）一律附上引用；專案自身的設計（架構、延遲目標、21 項驗收情境）標示為設計目標，而非實測結果。
- **不宣稱未完成的實驗結果**。IEEE 模板要求「不提供實驗結果」，因此實驗章節只寫規劃與目標值。
- **紅色佔位必須全部清掉再投稿**。過去版本曾用 `\textcolor{red}{[cite: ...]}` 標示待補來源，現在已全部替換（可用 `grep -r "textcolor{red}"` 確認）。

### 已修正的重要內容

| 原內容 | 問題 | 修正 |
|---|---|---|
| 「約 70% 健身 App 使用者 100 天內棄用」 | 來源是 JMIR 2024 scoping review，涵蓋多種生活型態與心理健康 App，中位數為 70% | 改為該研究的實際範圍，並寫明不是健身 App 專屬數字 |
| 「全球約 18 億成人身體活動不足」（來自 myRef 的提案解析） | 原文未標示年份與出處 | 改為 WHO 2024 新聞稿的數字：約 31%（約 18 億人），2022 年資料 |
| 「運動不足死亡風險高 20–30%」 | WHO fact sheet 的說法 | 保留，並引用 WHO fact sheet |
| 「LLM 與數位人成本高、安全有風險」 | 找不到來源 | 刪除，只保留有來源的延遲與算力描述 |
| 比較表的 Yes/No 判斷 | 找不到來源 | 改為每篇來源實際回報的能力，沒寫的標 `Not reported` |

## 三、Overleaf MCP 的安裝與使用

### 1. 設定檔

Claude Code 讀取的是**專案根目錄的 `.mcp.json`**。`.vscode/mcp.json` 是 VS Code（Copilot 等）的格式，Claude Code 不會載入它。本專案的 `.mcp.json` 內容如下：

```json
{
  "mcpServers": {
    "overleaf": {
      "type": "stdio",
      "command": "C:\\Program Files\\nodejs\\npx.cmd",
      "args": ["-y", "overleaf-web-mcp", "serve"]
    }
  }
}
```

### 2. 前置條件

- 已安裝 Node.js（確認 `C:\Program Files\nodejs\npx.cmd` 存在）。
- 套件 `overleaf-web-mcp` 可從 npm 取得（查詢時版本為 1.4.1）。

### 3. 啟用步驟

1. 在 `DIC4` 根目錄放好 `.mcp.json`。
2. **重新啟動 Claude Code 的 session**（關掉再開）。MCP server 是在 session 啟動時載入。
3. 第一次載入時若出現確認提示，選擇允許。
4. 執行 `/mcp` 檢查 `overleaf` 的狀態；若顯示失敗，錯誤訊息會說明原因。
5. 確認已登入 Overleaf（見下方「登入與 session」）。

### 4. 登入與 session

- 登入由 `overleaf-web-mcp` 自己處理，終端機可用 `npx overleaf-web-mcp login` 執行登入流程。
- Overleaf session 在五天沒有使用後會失效。可以排程執行 `npx overleaf-web-mcp keepalive` 維持 session。
- 用 `auth_status` 可以看到 session 的到期時間。

### 5. 常用的操作

| 目的 | 做法 |
|---|---|
| 列出專案 | `list_projects` |
| 查看檔案結構 | `get_project_tree` |
| 比對本機與 Overleaf 的差異（不修改） | `plan_sync`，給定本機資料夾路徑 |
| 同步到 Overleaf | `sync_directory`，`mode: additive`，`writeMode: tracked`，並帶入 `plan_sync` 回傳的 `planToken` |
| 編譯 | `compile_project`，看 `summary.builtCleanly` |
| 看 BibTeX 錯誤 | `get_compile_log`，`kind: bibtex` |

同步的安全設計：`plan_sync` 只比對不修改；`sync_directory` 必須帶入 plan 的 token，若專案在計畫後又被改動，會回傳 `REMOTE_DRIFT` 而不寫入。使用 `writeMode: tracked` 時，Overleaf 會把變更記成追蹤修改，可以在 Overleaf 的 Review 面板逐一接受或拒絕。

## 四、遇到的問題與解法

### 1. 本機沒有 `gh` CLI
推送與查詢都改用 `git` 與 GitHub 的 HTTPS／API。公開 repo 可以直接用 `curl https://api.github.com/...` 查詢；推送則依賴已存在的 git 憑證。

### 2. 本機資料夾不是 git repo
先 `git init`，並只在該 repo 內設定 `user.name` 與 `user.email`（使用 `git config` 不加 `--global`），避免改動全域設定。

### 3. 讀取 git 憑證被權限系統拒絕
嘗試用 `git credential fill` 查看已存的憑證時被拒絕。這是刻意保留的安全界線，我沒有繞過它。推送時若沒有認證提示，代表 git 已能使用既有憑證；若推送失敗，請在一般終端機執行 `git push` 完成登入。

### 4. Claude Code 讀不到 `.vscode/mcp.json`
重開 VS Code 後 MCP 仍不出現。原因是設定檔位置與格式不對，解法是建立根目錄的 `.mcp.json`，並重新啟動 session。

### 5. 本機沒有 LaTeX
不需要安裝 TeX Live 或 MiKTeX，改用 Overleaf 的 `compile_project` 遠端編譯。缺點是每次編譯會消耗 Overleaf 的編譯額度，不要重複編譯。

### 6. BibTeX 的作者格式錯誤
`author = {Kidman, Curtis, Watson, and Maher}` 會被 IEEEtran 解析成錯誤的名字結構，編譯報錯 `has a comma at the end`。解法是使用 `Last, First and Last, First` 格式，例如 `Kidman, Patrick G. and Curtis, Rachel G. and ...`。

### 7. bib 中的中文標題造成問題
pdfLaTeX 無法正確處理 bib 中的中文字。解法是改用英文翻譯，並標註 `[In Chinese]`，讓讀者知道原文是中文。

### 8. 模板錯誤
- `sections/03_proposed_scheme.tex` 寫入時曾多出一行 `\end{algorithmic}`，這行沒有對應的開始標籤，會造成編譯錯誤，已刪除（後續編譯已通過）。
- 架構圖的編號原本使用 `\textcircled{1}`，為避免依賴額外套件，改為 `(1)`。

### 9. 架構圖的檔案佔位
`figures/architecture.pdf` 不存在時，`\IfFileExists` 會顯示文字框。最後改用 TikZ 直接在 LaTeX 中畫出 8 模組架構圖，不需要外部圖檔；圖使用 `figure*` 跨欄，頁數因此從 4 頁變為 5 頁（仍在 ICEIB 的 4–8 頁範圍內）。

### 10. 網路搜尋的 WebFetch 工具消失
工作中途 `WebFetch` 不再可用。改用 `WebSearch` 取得摘要，並以 `curl` 抓取 GitHub 內容。

### 11. 可能遇到但本次未碰到的問題
- **瀏覽器登入異常**：若 Overleaf 或 GitHub 的瀏覽器登入流程卡住或反覆失敗（例如 Chrome 無法完成授權），可以改用帳號密碼登入，或改用工具的命令列登入流程。
- **MCP 未連上**：先確認 `.mcp.json` 位置、Node.js 路徑、session 是否已重啟，再查看 `/mcp` 的錯誤訊息。
- **Overleaf session 過期**：執行 `npx overleaf-web-mcp login` 重新登入。
- **同步出現 `REMOTE_DRIFT`**：代表 Overleaf 端在計畫後又被改動，重新執行 `plan_sync` 即可。
- **編譯額度耗盡**：等待後再編譯，或使用 `draft` 模式做文字檢查。

## 五、投稿前的檢查清單

- [ ] 在 Overleaf 打開 PDF，確認架構圖文字不超出框、箭頭清楚。
- [ ] 確認 `grep -r "textcolor{red}"` 沒有輸出。
- [ ] 確認 `compile_project` 的 `builtCleanly` 為 `true`，且無 undefined citation。
- [ ] 核對 `references.bib` 中的譯名與發布日期（公視新聞的標題為英文翻譯，發布日只確認到年份）。
- [ ] 確認作者單位與信箱（`National Chung Hsing University`、`Department of Computer Science and Engineering`）。
- [ ] 確認 ICEIB 2027 的頁數與格式要求（網站說明為 4–8 頁，最終以主辦單位為準）。
- [ ] 確認 Table I 的引用內容與原文一致，特別是 Jansons 的 115% 依從率與 Young 的準確度描述；這些多數來自搜尋摘要，尚未逐篇核對全文。

## 參考

- ICEIB 2027：https://www.iceib.asia/
- IEEE 模板：https://www.ieee.org/conferences/publishing/templates.html
- 組織 repo：https://github.com/orgs/115NCHUAIOT/repositories
- 個人 repo：https://github.com/samson8423-job/ICEIB2027

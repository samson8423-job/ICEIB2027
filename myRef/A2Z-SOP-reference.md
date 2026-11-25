# A2Z SOP 參考對照表（AI Fitness Coach）

> 來源：`A2Z outline example.pdf`（2020 A-Z SOP 定義）、`AI 健身教練提案解析.pdf`（既有解析稿）、`AI Fitness Coach 智慧健身資訊圖表.png`、`README.md`。
> 用途：撰寫提案／論文時，逐字母對照 A2Z SOP 的定義，並查看 AI Fitness Coach 專案對應的內容。

## 一、SOP 結構總覽

| 區段 | 字母 | 章節 |
|---|---|---|
| Abstract | A–F | 動機、挑戰、解法、設計、實驗、預期結果 |
| I. Introduction | G–L | 現況、瓶頸、文獻、評論、貢獻、章節安排 |
| II. Related Work | M–O | 既有方法分類、新提案、文獻比較歸納 |
| III. Proposed Scheme | PA–PM | 目標、基礎、解法、設計、元件、公式、架構圖、流程、範例、貢獻、模擬規劃、數學證明 |
| IV. Simulation | Q–V | 指標、比較方法、模擬設定、參數、結果、驗證 |
| V. Conclusion | W–Z | 回顧、獨特性、產出、結論與未來展望 |

## 二、逐字母對照

### Abstract（A–F）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| A | Attention Getter：動機及引題（科技進步、research interest） | 全民運動意識提升，約 18 億成年人身體活動不足；使用者想要用對話取得慢跑建議，而不是填寫多頁表單 |
| B | But：挑戰（However） | 健身 App 以手動紀錄為主，約 70% 使用者於百日內棄用；缺乏環境感知；語音延遲與雲端成本限制 |
| C | Cure：解藥（we propose a novel scheme） | 提出 AI Fitness Coach：以對話為唯一入口的慢跑教練網站，整合語音 Agent、數位人口型、天氣路線、食物辨識與 MCP 營養建議 |
| D | Development：方法設計（based on …） | 基於 CyborAgent（語音與工具迴圈）與 CyborAssistant（數位人與口型）；Vanilla JS + FastAPI；部署於 Vercel + Supabase；單一 FitnessCoachAgent + ScopeGuard |
| E | Experiments：實驗（we conduct experiments on …） | 端到端流程驗證：對話建議、天氣與路線、手動運動紀錄、食物熱量辨識、MCP 營養菜單、數位人口型同步、無關請求拒絕、中斷插話、資料隔離、雲端重現；採人工評分 + 自動化指標雙軌 |
| F | Findings：結果（results show that …） | **預期結果（Expected Outcomes）**：系統可在免費雲端環境完成主要流程，並以 ScopeGuard 與 Token 預算控制用量。提案階段尚未實測，不可寫成已完成的實驗結果 |

### Introduction（G–L）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| G | General：現況（with the explosive growth of …） | 穿戴裝置與 AI 普及，數位健身市場快速成長 |
| H | However：挑戰（同 B，但更長） | 表單負擔重；天氣、路線、飲食難以即時串接；免費雲端難同時兼顧低延遲與可部署性 |
| I | In literature：文獻解法（大分類，由舊到新、由簡到繁） | 傳統健身 App → 語音助理 → 姿態偵測 → LLM Agent 工具呼叫 → 即時語音口型數位人 |
| J | Judgement：各類方法優缺點（SWOT） | 表單式互動生硬；純語音助理缺乏視覺與環境工具；傳統數位人成本高。本案在免費優先前提下取得平衡 |
| K | Keypoint：設計精神與 novelty（重申主要貢獻） | (1) 對話入口降低認知負擔；(2) 整合天氣／路線／飲食／數位人；(3) 可公開連線的雲端部署；(4) ScopeGuard 與用量預算 |
| L | List the organization：文章結構 | 第 II 節相關研究、第 III 節提案方法、第 IV 節模擬驗證、第 V 節結論 |

### Related Work（M–O）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| M | Methods：既有方法分類（由舊到新、由簡到繁） | 傳統紀錄 App、語音助理型、姿態／感測分析、LLM Agent + 工具呼叫、即時語音與口型同步數位人 |
| N | New proposed：新趨勢與新穎性 | LLM 工具迴圈 + MCP 協議 + 低延遲 STT/TTS + 可嵌入的無框數位人，整合為可免費雲端運行的慢跑教練 |
| O | Organize：依 J 的評論做 SWOT 4×5 表歸納 | 比較互動自然度、環境感知、飲食整合、部署成本、數位人臨場感、用量可控性。**待補：Article Summary Table（見第四節）** |

### Proposed Scheme（PA–PM）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| PA | Aim：研究目標 | 開發可公開連線的 AI 慢跑教練；使用者以語音或文字詢問「今天怎麼跑」，系統調用工具產生可採用的計畫，並支援飲食紀錄 |
| PB | Based on：依託的基礎與背景 | CyborAgent（對話／語音／Agent）與 CyborAssistant（人像／口型）整併至主倉 `ai-fitness-coach` 並擴充慢跑業務邏輯 |
| PC | Cure / Cause：解法有效的原因 | 單一 FitnessCoachAgent + ScopeGuard 非 LLM 意圖過濾，避免多 Agent 的複雜度與成本失控；後端先驗證再呼叫模型 |
| PD | Design：系統架構設計 | 前端 Vanilla JS ES modules；後端 FastAPI（Vercel Functions）；Supabase Postgres／SQLite；REST + WebSocket；口型為獨立服務 |
| PE | Paper element：模組元件 | 六大交付模組：教練對話 Agent、天氣定位路線、跑步規劃與紀錄、食物熱量辨識、營養菜單 MCP、AI 人像與語音 |
| PF | Formulation：功能與數學形式 | 配速 `Pace = duration(s) / distance(km)`；用量預算原子更新；狀態轉移條件；Idempotency 雜湊 |
| PG | Graph：架構圖與流程圖 | Mermaid flowchart TB（系統架構）、sequenceDiagram（對話／計畫）、stateDiagram-v2（跑步狀態） |
| PH | How：如何達成設計目標 | 使用者輸入 → ScopeGuard（意圖與配額）→ TaskSpec → Agent 呼叫 Tool → 後端驗證 → 文字／卡片／TTS／口型輸出 |
| PI | Implementation：虛擬碼與元件互動 | REST 處理持久化業務資料；WebSocket 處理即時對話與語音串流；Tool Gateway 做權限驗證。**待補：帶行號的 Pseudocode（見第四節）** |
| PJ | Jump to an example：情境範例 | 「今天想跑 30 分鐘」→ 查天氣與路線 → 產生草案 → 使用者確認「採用並開始慢跑」→ 建立 active run → 結束後補登距離 |
| PK | Key points contribution：核心貢獻 | 對話入口整合、嚴格範圍與用量控制、免費雲端部署的可重現性、多模組主倉整併策略 |
| PL | Later how to do simulation：後續模擬 | 第一階段：展示區域路線與手動距離驗證流程；第二階段：擴充手機瀏覽器持續 GPS 與感測器 |
| PM | Math proof / Logic：數學與邏輯檢驗 | 配速計算邏輯、暫停時間排除規則、時區切日統計、Supabase 交易鎖定機制 |

### Simulation（Q–V）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| Q | Quality & Quantity：評估指標 | 一般頁面 P95 < 2 s；文字到首段回覆 P95 < 8 s；語音到可聽回覆 P95 < 12 s；停止播放 < 300 ms（**皆為提案中的體驗目標，非實測值**） |
| R | Related method to compare：比較方法 | 傳統表單健身 App、純文字 LLM Chatbot、無口型語音助理、本地高價 GPU 部署方案 |
| S | Simulation Setup：實驗環境 | 展示帳號、人工核對的校園／展示區域路線、免費雲端模型（Gemini、Groq、Azure）的 API 額度與退避機制 |
| T | Tuning parameters：調校參數 | 工具迭代上限 3 次、呼叫上限 6 次、工具逾時 10 s、推理預算 45 s、口型影片分段 1–2 s、每日 Token 配額 |
| U | Useful results：預期產出 | 慢跑路線地圖卡片、運動計時器、食物熱量辨識修正卡、MCP 營養菜單卡、無框 AI 數位人互動 |
| V | Verify：有效性驗證 | 21 項驗收情境（含拒絕無關請求、中斷插話、資料隔離、雲端重現） |

### Conclusion（W–Z）

| 字母 | SOP 定義 | AI Fitness Coach 內容 |
|---|---|---|
| W | What proposed：回顧研究意義與挑戰 | 解決慢跑習慣建立中「表單繁瑣、缺乏環境感知、部署成本高」的問題，提出對話式 Domain AI Agent |
| X | eXcel：專精情境與優勢 | 慢跑情境對話規劃、免費雲端零成本運行、ScopeGuard 嚴格用量防護、無框口型數位人互動 |
| Y | Yields：產出與結果 | 預期交付：GitHub repo、README.md、DESIGN.md、公開連線網址、Demo 影片、Proposal 簡報 |
| Z | Zen：結論與 future work | 第二階段：手機瀏覽器持續 GPS、步數與動作姿態感測、更低成本的口型生成、長期記憶、更大規模的使用者實驗 |

## 三、來源之間的差異（撰寫前請確認）

1. **字母定義不一致**：`AI 健身教練提案解析.pdf` 把 G–Z 的定義用在 Introduction／Conclusion，而 `AI Fitness Coach 智慧健身資訊圖表.png` 右上角的「A2Z SOP 26 字母對應表」對 N–Z 的定義不同（例如 N 為 Novelty、O 為 Overview、P 為 Plan、Q 為 Question、R 為 Research、S 為 Summary、T 為 Technique、U 為 Utility、V 為 Validation、W 為 What-if、X 為 eXample、Y 為 Yield、Z 為 Conclusion）。**本檔以 `A2Z outline example.pdf` 的原始定義為準。** 若要沿用資訊圖表的版本，需先與老師或範例確認。
2. **Expected Outcomes**：F、Q、U 等都是提案階段的預期內容。除非實測完成，不可寫成已驗證結果。
3. **README 的 Repository Contents** 標示為 `AIoT-DIC3/`，與本專案名稱（`ICEIB2027`／DIC4）不符，提交前建議更新。

## 四、待補項目（依規範要求）

- [ ] **Article Summary Table**（O／M 區）：欄位為 Ref. | Year | Problem Limitation | AI Method | Domain Knowledge | Dataset | Findings，並由此導出 Research Gap。
- [ ] **帶行號的 Pseudocode**（PI 區）：例如 Algorithm 1: FitnessCoachAgent Loop，行號 01–12。目前 design.md 只有 Mermaid 圖。
- [ ] **Deep Research 文字**（G–K 或 PJ 前後）：5 個核心思維模式、3 個專業爭論、10 個深度問題（Why、What-if、Trade-off、Failure Case、Evidence、Comparison、User Impact、Scalability、Ethics、Future）。
- [ ] **架構圖編號**：在 PG 的系統架構圖上標 ①–⑧，並在正文逐一說明。
- [ ] **正式的引用文獻**：WHO、公視新聞、JMIR、PubMed 等來源需補完整 bibliographic citation。

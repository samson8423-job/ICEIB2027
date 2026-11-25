# 專案背景：AI Fitness Coach

## 目的

AI Fitness Coach 是以對話為唯一入口的 Domain AI Agent 慢跑教練網站。使用者以文字或語音詢問（例如「今天想跑 30 分鐘」），系統整合天氣、路線、運動紀錄、食物辨識與營養建議，產生可確認的慢跑計畫。

專案同時產出 ICEIB 2027 IEEE 論文（`ICEIB2027-IEEE/`）。

## 技術棧（設計目標）

- 前端：Vanilla JavaScript（ES modules），部署於 Vercel CDN。
- 後端：FastAPI，部署為 Vercel Functions；REST 處理持久化資料，WebSocket 處理即時對話與語音串流。
- 資料庫與儲存：Supabase Postgres 與 Supabase Storage。
- 模型：LLM、STT、TTS、Vision 皆為雲端 HTTPS 端點；口型為獨立的雲端服務。
- 外部資料：中央氣象署（CWA）天氣、Leaflet／Geoapify 地圖與路線、營養 MCP server。

## 約束

- 第一階段以免費方案為目標（Vercel Hobby、Supabase 免費額度、免費模型額度）。超額時提示服務受限，不自動升級。
- 單一 `FitnessCoachAgent` 搭配非 LLM 的 `ScopeGuard`，避免多 Agent 的複雜度與成本失控。
- 所有保存動作都在後端重新驗證，不信任前端或模型的計算結果。

## 來源

- 組織 repo：`115NCHUAIOT/ai-fitness-coach`（`design.md` 為架構與部署的權威文件，`proposal.md` 為提案）。
- 個人 repo：`samson8423-job/ICEIB2027`（論文與本工作紀錄）。

## 論文工作慣例

- 論文以 IEEEtran 撰寫，頁數依 ICEIB 2027 規定（網站說明為 4–8 頁，以主辦單位為準）。
- 外部事實一律附引用，引用條目放在 `references.bib`。
- 專案自身的設計以「設計目標」描述，不寫成實測結果。
- 修改後以 Overleaf MCP 同步（`plan_sync` → `sync_directory` 的 tracked 模式）並編譯，確認 `builtCleanly` 為 `true`。

## 工具

- Overleaf MCP：`.mcp.json` 設定，見根目錄 `README.md` 第三節。
- Git：推送目標為 `samson8423-job/ICEIB2027` 的 `main` 分支。

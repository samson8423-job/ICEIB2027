# 變更提案：完成實驗評估

## 為什麼

目前論文只有設計與評估規劃，沒有任何實測結果。第 IV 節的延遲與依從率等數字都是目標值。要讓論文具備投稿價值，需要實際量測，並把結果寫入第 IV 節。

同時，部分文獻數字（例如 Jansons 2022 的依從率、Young 2023 的準確度）來自搜尋摘要，尚未逐篇核對全文。

## 改變什麼

1. 依 `specs/scope-guard`、`specs/coach-agent`、`specs/token-budget` 的規格，建立可重現的測試情境。
2. 量測第 IV 節 Q 所列的指標：頁面 P95、文字到首段回覆 P95、語音到可聽回覆 P95、停止播放時間。
3. 以 Token 用量與 ScopeGuard 拒絕率比較「有／無 ScopeGuard 與預算」的設定（對應第 IV 節的消融規劃）。
4. 逐篇核對 Table I 所引用的文獻全文，修正數字與描述。
5. 把結果寫入 `04_experiments.tex`，並把目標值改為實測值。

## 影響範圍

- `ICEIB2027-IEEE/sections/04_experiments.tex`
- `ICEIB2027-IEEE/sections/02_related_work.tex`（Table I）
- `ICEIB2027-IEEE/references.bib`
- 後端實作與測試（不在本 repo，見 `115NCHUAIOT/ai-fitness-coach`）

## 不在範圍內

- 手機瀏覽器持續 GPS 追蹤、步數與姿態感測（列為未來工作）。
- 長期使用者研究。

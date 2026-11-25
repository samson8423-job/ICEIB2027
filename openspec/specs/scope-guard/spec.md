# ScopeGuard 規格

## Requirements

### Requirement: 非 LLM 的意圖篩選
ScopeGuard MUST 在呼叫任何語言模型之前，以非 LLM 的規則判斷使用者輸入是否屬於慢跑、運動紀錄、天氣、路線與飲食的範圍。

#### Scenario: 範圍內的請求
- **WHEN** 使用者輸入「今天想跑 30 分鐘」
- **THEN** ScopeGuard MUST 接受該請求，並產生 TaskSpec 交給 FitnessCoachAgent

#### Scenario: 範圍外的請求
- **WHEN** 使用者輸入與運動無關的請求
- **THEN** ScopeGuard MUST 拒絕該請求，且不呼叫語言模型

### Requirement: 配額檢查先於模型呼叫
ScopeGuard MUST 在模型呼叫前檢查該使用者當日的 Token 配額，配額不足時 MUST 回傳配額訊息。

#### Scenario: 配額不足
- **WHEN** 使用者當日已用盡 Token 配額
- **THEN** 系統 MUST 回傳配額訊息，且不產生任何模型呼叫

## 相關文件

- 本規格對應 `ICEIB2027-IEEE/sections/03_proposed_scheme.tex` 的 Algorithm 1 第 1–4 行。
- 架構位置與部署依 `design.md` 第 4 節。

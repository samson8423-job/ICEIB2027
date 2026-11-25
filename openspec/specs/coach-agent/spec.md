# FitnessCoachAgent 規格

## Requirements

### Requirement: 有界的工具迴圈
FitnessCoachAgent MUST 以有界的工具迴圈運作，每次回合的上限如下：工具迭代最多 3 次、工具呼叫最多 6 次、整體推理預算 45 秒。

#### Scenario: 達到上限
- **WHEN** 工具迭代次數達到 3 次，或工具呼叫次數達到 6 次，或推理時間達到 45 秒
- **THEN** Agent MUST 停止迴圈，並以目前的結果產生回覆

#### Scenario: 工具逾時
- **WHEN** 單一工具呼叫超過 10 秒
- **THEN** 系統 MUST 中止該呼叫，並將逾時結果回報給 Agent

### Requirement: 工具結果經後端驗證
Tool Gateway MUST 在工具結果交給模型或使用者之前，驗證身分與參數。模型指定的任意 SQL、URL 或 user_id MUST NOT 被執行。

#### Scenario: 模型要求非法的使用者資料
- **WHEN** 模型要求讀取其他使用者的資料
- **THEN** Tool Gateway MUST 拒絕該要求，且不回傳任何資料

### Requirement: 單一回答管線
語音與文字輸入 MUST 共用同一條教練回答管線。系統 MUST NOT 同時執行第二套 LLM 或 TTS 產生另一份回答。

#### Scenario: 語音輸入
- **WHEN** 使用者以語音提問
- **THEN** 系統 MUST 先經 STT 轉為文字，再進入同一條 Agent 管線，並以同一份 TTS 音訊驅動口型

## 相關文件

- 本規格對應 `ICEIB2027-IEEE/sections/03_proposed_scheme.tex` 的 Algorithm 1。
- 模組邊界依 `design.md` 第 4.1 節。

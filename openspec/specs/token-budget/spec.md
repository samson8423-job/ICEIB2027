# Token 預算規格

## Requirements

### Requirement: 每日 Token 預算
每位使用者的每日模型用量 MUST NOT 超過該日的 Token 預算 $B_d$。每次模型呼叫的用量 $\tau_i$ MUST 在呼叫前預留，並在呼叫後結算。

#### Scenario: 預留成功
- **WHEN** 剩餘預算足以支應本次呼叫的預估用量
- **THEN** 系統 MUST 預留用量並執行呼叫

#### Scenario: 預留失敗
- **WHEN** 剩餘預算不足以支應本次呼叫
- **THEN** 系統 MUST 不執行呼叫，並回傳配額訊息

### Requirement: 預留與結算為原子操作
預留與結算 MUST 由資料庫交易完成，不依賴單一服務實例的記憶體狀態，以避免並行請求造成超額。

#### Scenario: 並行請求
- **WHEN** 同一使用者同時送出兩個請求，且兩者合計超過剩餘預算
- **THEN** 系統 MUST 只允許不超過預算的請求執行

## 相關文件

- 公式與約束見 `ICEIB2027-IEEE/sections/03_proposed_scheme.tex` 的式 (2)。
- 免費方案的計量與限流規則見 `design.md` 第 6.5 節。

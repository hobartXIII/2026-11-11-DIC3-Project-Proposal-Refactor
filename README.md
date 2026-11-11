# DIC3 Project Proposal Refactor — OpenSpec

> 本 Repository 用於整理 DIC3 課程的 Project Proposal Refactor 作業，並以 OpenSpec / Spec-Driven Development（SDD）管理提案重構流程。  
> **重要：DietSense 與 AirGuard AI 是不同階段的研究提案，不是同一個系統的前後版本。**

## 1. 專案定位與兩階段研究脈絡

本專案保留課程前期提案與後續研究提案的脈絡，並以 OpenSpec 將研究構想轉化為可追蹤、可驗證的規格。

### Stage 1 — DietSense：熱量 AI Agent（前期課堂提案）

DietSense 是前期課堂提出的熱量 AI Agent 構想，聚焦飲食內容理解與熱量資訊整理。此階段的簡報、資訊圖表、提案文件與課程附件，作為研究構想發展的起點與課堂歷程紀錄。

### Stage 2 — AirGuard AI：空氣品質預測與戶外運動決策支援 AI Agent（後續研究提案）

AirGuard AI 是後續發展的獨立研究題目，聚焦空氣品質資料、短期預測、官方健康指引與工具導向 AI Agent。系統希望協助民眾在慢跑等戶外活動前，理解未來短時間的空品變化與活動風險。

| 比較面向 | Stage 1：DietSense | Stage 2：AirGuard AI |
|---|---|---|
| 研究主題 | 飲食與熱量資訊 | 空氣品質與戶外活動決策 |
| AI 應用 | 熱量 AI Agent | 空品預測與決策支援 Agent |
| 階段定位 | 前期課堂提案 | 後續研究提案 |
| 本 Repository 的用途 | 保存提案背景與課堂產出 | 作為本次 Proposal Refactor 的主要研究題目 |

## 2. DIC3 作業目標

本次作業以 OpenSpec 練習 Spec-Driven Development，將原本以自然語言描述的研究構想，整理成明確、可追蹤且可驗證的規格。

1. 安裝並熟悉 OpenSpec，理解 Spec-Driven Development 的工作方式。
2. 釐清專題 A–F（Abstract / Proposal）架構。
3. 定義研究的 **5 個核心（Cores）**、**3 個取捨（Trade-offs）** 與 **10 個具體細節（Details）**。
4. 將既有 `design.md` 的內容轉換成 OpenSpec workflow。
5. 重構 Introduction，使研究背景、問題、動機與貢獻更清楚。
6. 重構 Related Work，建立主題分類、研究缺口與本研究定位。

## 3. AirGuard AI：A–F 提案架構

### A — Attention Getter：研究情境與問題重要性

民眾進行慢跑、散步或其他戶外活動時，可能受到空氣污染影響。即使已有空氣品質監測與預報資訊，使用者仍需要自行查找資料、判讀 AQI，並決定是否調整活動時間或地點。若資訊無法轉化成容易理解的行動建議，便難以直接支援日常決策。

### B — But：現有方法的不足與研究缺口

現有服務多提供即時監測、歷史觀測或區域預報，使用者仍需自行整合不同資訊並解讀其意義。單純呈現 AQI 數值，也未必能回答「接下來一段時間是否適合戶外運動」等情境問題。

另一方面，若直接交由大型語言模型（LLM）自由生成建議，可能產生幻覺、引用不明確，或超出一般資訊服務範圍而提供不恰當的醫療判斷。因此，本研究需要兼顧預測依據、官方指引、來源透明與安全邊界。

### C — Cure：提出的解決方案

建置整合官方空氣品質資料、短期時間序列預測、官方指引規則與工具導向 AI Agent 的決策支援系統。Agent 透過明確工具取得預測結果及規則資料，再將結果整理為可理解的戶外活動資訊，並揭露資料來源與限制。

### D — Development：系統實作規劃

- **Backend：** 使用 FastAPI 建置服務 API。
- **Agent orchestration：** 使用 LangChain 與受白名單限制的 Function Calling，讓 LLM 僅能呼叫定義好的工具。
- **Forecasting：** 建立短期空氣品質時間序列預測模型，並與基線方法比較。
- **Guideline rules：** 將環境部官方 AQI 分級與相關指引整理為可查詢的規則庫。
- **Grounding：** Agent 回答須以工具回傳資料及規則為依據，不允許模型自行捏造即時數值或官方規範。

### E — Experiments：實驗與評估規劃

1. 蒐集並整理 2024–2026 年臺中測站空氣品質觀測資料（實際可用期間依資料完整性確認）。
2. 採用 rolling time-series backtest，依時間先後切分訓練與測試資料，避免使用未來資訊。
3. 以 MAE、RMSE 等指標評估短期預測表現，並與適當基線模型比較。
4. 規劃至少 30 個 Agent 測試情境，涵蓋正常查詢、資料缺漏、工具失敗、來源追問、錯誤假設及醫療越界等情境。
5. 評估回答是否正確引用工具結果、揭露資料來源、遵循官方規則，以及在資料不足或工具失敗時安全降級。

### F — Findings：預期成果與研究貢獻

本階段屬於研究提案與實作規劃，尚未取得的實驗結果不得寫成已驗證的研究發現。預期成果包括：一套結合短期空品預測與官方指引的 Agent 原型、一套可重現的時間序列回測流程，以及一組用於檢驗 Agent 正確性、可追溯性與安全邊界的測試案例。最終研究貢獻須依實際實驗結果修正與確認。

## 4. Five Cores — 五個核心

1. **Data Integration：官方資料整合。** 整理環境部空氣品質觀測資料，保留測站、時間、污染物與資料來源等必要欄位。
2. **Short-term Forecasting：短期預測。** 以時間序列方法預測指定測站或區域的短期空品變化，並透過時間序列回測驗證。
3. **Official Guideline Rules：官方指引規則。** 將 AQI 分級與官方健康指引整理成結構化規則，作為建議依據。
4. **Tool-grounded Agent：工具依據型 Agent。** 透過白名單工具取得資料與規則，LLM 負責理解問題及組織回答，不自行編造數據。
5. **Safety & Traceability：安全與可追溯性。** 顯示資料時間與來源；資料不足、工具失敗或問題超出範圍時，明確說明限制並安全降級。

## 5. Three Trade-offs — 三個設計取捨

| Trade-off | 取捨與考量 |
|---|---|
| 預測效能 vs. 可解釋性 | 複雜模型可能提升預測能力，但需兼顧資料需求、可解釋性與維護成本；應以基線及回測結果支持模型選擇。 |
| LLM 彈性 vs. 安全可控 | 自由生成較有彈性，但風險較高；採用白名單 Function Calling、規則查詢與受限任務範圍降低幻覺及越界風險。 |
| 即時性 vs. 資料可靠性 | 越即時的資料整合可能增加延遲、缺漏與來源更新差異；應揭露資料時間，並在資料過期或缺漏時避免產生確定性建議。 |

## 6. Ten Details — 十項待明確化細節

1. **資料來源：** 環境部公開空氣品質資料及官方 AQI 指引；記錄資料集名稱、欄位、更新頻率與授權條件。
2. **研究範圍：** 以臺中測站或明確選定的測站集合為研究範圍，避免在未定義空間尺度時宣稱可代表整個城市。
3. **資料期間：** 規劃使用 2024–2026 年資料，實際起訖日期與缺漏情形須經下載及檢查後確認。
4. **預測目標：** 明確指定預測的污染物或 AQI 指標、測站，以及預測時間跨度。
5. **預測視窗：** 定義預測提前量、輸入歷史視窗、資料頻率與更新方式。
6. **基線模型：** 選擇合理的簡單基線（例如持續性預測或其他適用方法），並確保比較條件一致。
7. **Rolling backtest：** 依時間順序逐步移動訓練與測試視窗，避免隨機切分造成時間資訊洩漏。
8. **評估指標：** 使用 MAE、RMSE 等回歸指標；必要時另行定義不同污染物或測站的分組評估方式。
9. **Agent 情境測試：** 至少規劃 30 個案例，涵蓋正常流程、錯誤輸入、資料缺漏、工具異常、來源查詢與醫療越界。
10. **來源揭露與安全降級：** 回答顯示資料來源與時間；若資料不可用、過期或超出系統能力，須拒絕推測並說明限制。

## 7. Abstract — 約 500 字摘要草稿

隨著民眾對戶外運動與健康生活的重視，空氣品質已成為安排慢跑、散步等活動時的重要考量。然而，現有空氣品質服務多以即時監測、歷史資料或區域預報為主，使用者仍須自行判讀 AQI 數值、整合不同資訊，才能決定是否調整活動時間。另一方面，大型語言模型雖具備自然語言理解與對話能力，若缺乏可靠資料來源及明確規則約束，可能產生錯誤資訊、引用不明或超出一般資訊服務範圍的建議。

本研究提出 AirGuard AI，一套結合空氣品質短期預測與工具導向 AI Agent 的戶外運動決策支援系統。系統規劃整合環境部公開空氣品質觀測資料，建立短期時間序列預測模型，並將官方 AQI 分級及相關指引整理為結構化規則。後端以 FastAPI 提供服務，透過 LangChain 與白名單 Function Calling 限制 Agent 可使用的工具，使大型語言模型依據預測結果及官方規則組織回答，而非自行生成未經查證的空品數值或健康規範。

研究評估分為預測模型與 Agent 兩個層面。預測模型預計使用 2024 至 2026 年臺中測站資料，採 rolling time-series backtest，並以 MAE、RMSE 等指標與基線方法比較；Agent 則規劃至少 30 個測試情境，檢驗資料缺漏、工具失敗、來源追問及醫療越界等狀況下的回答正確性、來源揭露與安全降級能力。預期本研究可建立一套兼顧預測效能、資訊可追溯性與安全控制的空品決策支援原型，協助使用者更容易理解空氣品質資訊並安排戶外活動。實際研究結論將依系統實作及實驗結果確認。

## 8. OpenSpec / Spec-Driven Development Workflow

本 Repository 以規格先行的方式管理需求、設計與實作。每次變更應先釐清問題與驗收條件，再進行設計及實作，最後確認規格與成果一致。

建議工作流程：

1. **Explore：** 閱讀現有提案、`design.md`、作業規範與相關文獻，確認範圍及既有決策。
2. **Specify：** 將需求整理為明確的功能、非功能需求、限制與驗收情境。
3. **Plan：** 定義技術方案、資料流程、模型評估方式、Agent 工具邊界與風險處理。
4. **Tasks：** 將工作拆成可執行且可檢查的任務。
5. **Implement：** 依核准的規格實作，不任意擴張研究範圍。
6. **Validate：** 以測試、回測、文件檢查及驗收條件確認成果。
7. **Archive / Update：** 將完成的變更歸檔，並同步更新規格與研究文件。

> OpenSpec 的實際指令與目錄結構，應以安裝版本及官方文件為準；不要將此處的概念流程誤認為所有版本皆完全相同的固定 CLI 指令。

### 從 design.md 轉換至 OpenSpec

| 既有 design.md 內容 | OpenSpec 整理方向 |
|---|---|
| 研究背景與問題 | Proposal / Introduction 的背景與需求 |
| 系統目標與範圍 | Scope、Goals、Non-goals |
| 系統架構與模組 | Design / Architecture |
| 資料來源與處理流程 | Data requirements、Interfaces |
| 預測模型與評估 | Model requirements、Evaluation scenarios |
| Agent 工具與規則 | Tool contracts、Safety constraints |
| 錯誤處理與限制 | Failure modes、Fallback behavior |
| 預期成果與驗收 | Acceptance criteria |

轉換時應保留原始研究意圖，將模糊敘述改寫為可驗證條件；若內容尚未決定，標記為待確認，不應自行補成既定事實。

## 9. Introduction 重構指引

Introduction 建議依下列脈絡編排：

1. **研究背景：** 說明空氣品質與戶外活動決策的關係。
2. **實務問題：** 說明使用者需要整合即時資訊、預報及活動情境。
3. **現有方法限制：** 說明單純監測呈現、預報服務與自由生成式 LLM 的不足。
4. **研究缺口：** 聚焦短期預測、官方指引、工具依據及安全可追溯性之整合。
5. **研究目的：** 清楚列出 AirGuard AI 要解決的問題與研究範圍。
6. **研究貢獻：** 以可驗證方式描述資料整合、預測評估、Agent 設計及安全測試。
7. **文章組織：** 簡述後續章節安排。

避免在 Introduction 中提前宣稱尚未完成的預測準確率、使用者效益或實驗成功結果。

## 10. Related Work 重構指引

Related Work 不宜只依文獻逐篇摘要，建議依研究主題分類，並在每一類最後整理與本研究的關係。

| 文獻主題 | 建議整理重點 | 與 AirGuard AI 的關係 |
|---|---|---|
| 空氣品質監測與資料服務 | 官方監測資料、測站特性、資料更新與缺漏 | 建立資料來源與使用限制 |
| 空氣品質預測 | 時間序列模型、預測提前量、基線與評估指標 | 支持預測方法選擇 |
| 空品指標與健康指引 | AQI 分級、官方建議、適用範圍 | 建立規則庫與建議依據 |
| LLM / Tool Calling Agent | 工具使用、檢索、結構化輸出與幻覺控制 | 支持 Agent 架構 |
| AI 安全與可信任性 | 來源揭露、錯誤處理、越界控制與可追溯性 | 建立安全測試及降級機制 |

建議建立文獻矩陣，欄位包含：作者與年份、研究問題、資料集、方法、評估指標、主要發現、限制，以及與本研究的關聯。研究缺口應由文獻比較推導，不應僅以「目前沒有相關研究」作為結論。

## 11. 建議 Repository 目錄

以下為文件管理建議，可依實際 OpenSpec 初始化結果調整：

```text
.
├── README.md
├── design.md
├── proposal/
│   ├── abstract.md
│   ├── introduction.md
│   ├── related-work.md
│   └── references.md
├── research/
│   ├── five-cores.md
│   ├── trade-offs.md
│   ├── ten-details.md
│   ├── experiment-plan.md
│   └── evaluation-matrix.md
├── openspec/
│   ├── specs/
│   └── changes/
└── materials/
    └── stage-1-dietsense/
```

## 12. Stage 1 DietSense 課堂附件與資料

以下附件屬於 Stage 1 DietSense 前期課堂提案，應作為歷程資料保存，不應誤標為 AirGuard AI 的研究成果。

| 檔案 | 說明 | LMS |
|---|---|---|
| NBLM output 熱量AI提案解析.pdf | 提案解析 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928107) |
| DietSense資訊圖表.png | 提案資訊圖表 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928108) |
| DietSense_Proposal_Presentation.pptx | 提案簡報 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928109) |
| 期中Proposal_熱量AI_Agent.docx | 期中提案文件 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928110) |
| A2Z outline example.pdf | 提案架構參考 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928111) |
| 期末專題提案作業規範.pdf | 課程作業規範 | [下載](https://lms2020.nchu.edu.tw/filedownload/2928112) |

以上為課程提供的 LMS 連結，存取可能需要校內帳號或課程權限。

## 13. 參考資源

- [OpenSpec GitHub Repository](https://github.com/Fission-AI/OpenSpec)
- [OpenSpec 官方文件](https://openspec.dev/)
- [環境部環境資料開放平臺](https://data.moenv.gov.tw/)
- [環境部空氣品質監測網](https://airtw.moenv.gov.tw/)

## 14. 目前待辦事項

- [ ] 安裝 OpenSpec 並確認版本及初始化方式。
- [ ] 整理 Stage 1 DietSense 附件與課程歷程。
- [ ] 確認 Stage 2 AirGuard AI 的研究範圍、測站與預測目標。
- [ ] 將 `design.md` 轉換成 OpenSpec 規格與變更任務。
- [ ] 完成 Introduction 重構及引用查核。
- [ ] 完成 Related Work 文獻蒐集、分類與研究缺口整理。
- [ ] 確認資料期間、缺漏情形、基線模型與 rolling backtest 設計。
- [ ] 建立至少 30 個 Agent 測試情境及驗收標準。
- [ ] 僅在實驗完成後填入實際結果與研究發現。

---

**研究誠信聲明：** 本文件描述的是提案規劃與預期成果。所有尚未實際執行的模型效能、Agent 測試結果及使用者效益，均不得表述為已完成或已驗證的研究發現。

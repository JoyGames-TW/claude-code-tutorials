# 03-2-2 文件處理類：docx/pdf 規格書摘要與結案報告產出

> ⚠️ **線上核實狀態**：已核實（2026-06-06）。文件處理策略（正向摘要 + 反向報告）正確。pandoc 轉換指令可用。
> **注意**：Claude Code 對 PDF/docx 的原生讀取支援程度隨版本而異。若不支援直接讀取，使用 pandoc 轉換為 Markdown 是可靠的替代方案。

## 1. 本章學習目標

- 學會使用 Claude Code 讀取與處理 docx/pdf 格式的文件
- 掌握讓 Claude 摘要規格書、產出結案報告的技巧
- 理解結構化文件（docx/pdf）與純文字文件在 AI 處理上的差異
- 建立文件驅動的專案管理習慣

## 2. 適用對象與前置知識

- **適用對象**：需要處理大量文件的 PM、技術主管、需要撰寫結案報告的工程師
- **前置知識**：基本 Claude Code 操作、基本文件處理概念
- **關聯章節**：前接 [03-2-1 firecrawl](./03-2-1-firecrawl-docs-api-research.md)

## 3. 核心概念

### 3.1 Claude Code 的文件處理能力

Claude Code 可以透過 `@` 參照讀取多種格式的文件：
- `.md`（原生支援）
- `.txt`（原生支援）
- `.docx`（需轉換或透過 MCP）
- `.pdf`（需轉換或透過 MCP）

### 3.2 文件處理的典型場景

| 場景 | 輸入 | 輸出 | Claude 的角色 |
|------|------|------|-------------|
| 規格書摘要 | 客戶提供的 PDF 規格書 | spec.md | 提取關鍵需求，轉化為開發規格 |
| 結案報告 | spec.md + Git Log | 結案報告 docx | 整合開發歷程，產出正式文件 |
| API 文件 | 後端程式碼 | API 文件 md | 從程式碼反向產生文件 |

## 4. 操作步驟

### 4.1 讀取 PDF 規格書並摘要

**方法一：使用 pandoc 轉換為 Markdown 後讀取（最可靠）**
```bash
# 將 PDF 轉換為 Markdown
pandoc 客戶需求規格書.pdf -o 客戶需求規格書.md

# 將 docx 轉換為 Markdown
pandoc 需求文件.docx -o 需求文件.md
```

然後在 Claude Code 中：
```
請讀取 @客戶需求規格書.md，為我產出一份結構化的 spec.md。
重點提取：
1. 功能需求（What）
2. 非功能需求（效能、安全）
3. 資料模型暗示
4. 驗收條件
```

### 4.2 產出結案報告

```
請根據以下資料，為我產出一份結案報告：

參考文件：
- @spec.md（原始規格）
- @git:main..develop（開發過程的 Commit 歷史）

結案報告結構：
1. 專案概述
2. 完成功能清單（對照 spec.md）
3. 未完成項目與原因
4. 技術債清單
5. 部署說明
6. 後續建議

輸出格式：Markdown（可後續轉換為 docx）
```

### 4.3 常見的轉換指令

```bash
# 將 Markdown 轉換為 docx（使用 pandoc）
pandoc report.md -o report.docx --reference-doc=template.docx

# 將 docx 轉換為 Markdown 供 Claude 讀取
pandoc input.docx -o output.md
```

## 5. 常見錯誤與最佳實務

### 常見錯誤
1. **PDF 中的表格或圖片無法被正確解析**：使用 OCR 或手動轉換為文字
2. **規格書摘要過於簡略**：遺漏了非功能需求
3. **結案報告只描述「做了什麼」，未說明「為什麼」**

### 最佳實務
1. 讓 Claude 先產出摘要，人工確認後再產出完整文件
2. 結案報告要對照原始規格，逐條標註完成狀態
3. 技術債不應隱藏——誠實記錄，附上修正計畫
4. 文件處理的 Prompt 要明確指定輸出結構與格式

## 6. 小結

Claude Code 可以輔助處理 docx/pdf 文件，核心價值在於將非結構化文件轉化為結構化規格（正向），或將開發歷程整合為正式報告（反向）。

## 7. 延伸練習

1. 找一份真實的 PDF 規格書，讓 Claude 摘要為 spec.md
2. 根據你的專案 Git Log，讓 Claude 產出一份 Sprint 結案報告

## 8. 查核來源與版本備註

- 來源：pandoc 官方文件、Anthropic Claude Code 文件處理能力說明
- 查核日期：2026-06-06（已核實）
- 版本備註：Claude Code 對 PDF/docx 的原生支援程度以最新版本為準；若不支援，pandoc 轉換為 Markdown 後使用 `@` 參照是可靠的替代方案

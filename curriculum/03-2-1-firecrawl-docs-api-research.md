# 03-2-1 外部資料補充：網頁爬取最新文件與 API

> ⚠️ **線上核實狀態**：已核實（2026-06-06）。AI 知識時效性問題的分析正確，網頁爬取的最佳實務（robots.txt、範圍限制）為通用標準。
> **注意**：「firecrawl」MCP Server 的套件名稱、安裝方式與可用性需以 Claude Code MCP 生態系最新狀態為準。
> 本章的「何時該查、何時不該查」的判斷框架是通用的，不受特定工具影響。

## 1. 本章學習目標

- 理解 firecrawl MCP Server 的用途與運作原理
- 學會在 Claude Code 中設定與使用 firecrawl 來爬取網頁文件
- 掌握如何讓 Claude 自動查詢最新官方文件，而非依賴訓練資料中的過時資訊
- 理解何時該用 firecrawl、何時不該用（成本、合法性、必要性）
- 建立「先查最新文件，再寫程式碼」的 AI 開發習慣

## 2. 適用對象與前置知識

- **適用對象**：需要參考最新 API 文件或第三方文件的開發者
- **前置知識**：Claude Code MCP 設定（01-4-3）、基本 HTTP 與網頁概念
- **關聯章節**：後接 [03-2-2 docx/pdf 處理](./03-2-2-docx-pdf-summary-and-report-generation.md)

## 3. 核心概念

### 3.1 AI 的知識時效性問題

Claude 的訓練資料有截止日期。當你問 Claude 關於某個 Library 的最新 API 用法時，它可能基於舊版本文檔回答。firecrawl 解決了這個問題——讓 Claude 能即時爬取最新官方文件。

### 3.2 firecrawl 的運作模式

```mermaid
flowchart LR
    A["開發者提問"] --> B["Claude 判斷\n需要最新文件"]
    B --> C["透過 firecrawl MCP\n爬取目標網頁"]
    C --> D["提取網頁內容\n轉為 Markdown"]
    D --> E["Claude 讀取內容\n回答問題"]
```

## 4. 操作步驟

### 4.1 在 CLAUDE.md 中宣告 firecrawl MCP

```json
{
  "mcpServers": {
    "firecrawl": {
      "command": "npx",
      "args": ["-y", "@anthropic/mcp-server-firecrawl"],
      "description": "網頁爬取 MCP Server，用於查詢最新 API 文件"
    }
  }
}
```

### 4.2 使用 firecrawl 查詢文件

```
Spring Boot 3.3 的 @PreAuthorize 有新增什麼用法嗎？
請先用 firecrawl 查詢 Spring Security 最新官方文件，再回答。
```

### 4.3 使用時的注意事項

- 僅爬取公開的官方文件網站
- 遵守目標網站的 robots.txt
- 不要用 firecrawl 爬取需要認證的頁面
- 注意 API 使用量與成本

## 5. 常見錯誤與最佳實務

### 常見錯誤
1. **過度依賴爬取**：每次問問題都先爬取，浪費時間與資源。AI 的訓練資料對主流框架（如 Spring Boot、React）的基礎 API 通常足夠準確
2. **爬取非官方來源**：爬取了過時或錯誤的第三方部落格，而非官方文檔
3. **未指定爬取範圍**：爬取了整個網站而非特定頁面，導致 Context 被不必要的內容佔滿

### 最佳實務
1. 先用 Claude 的既有知識，不確定時才查最新文件
2. 指定爬取範圍（特定 URL 或頁面）
3. 將爬取結果記錄在對話中，供後續參考
4. 遵守 robots.txt 與網站使用條款

### 判斷何時該查最新文件的決策框架

| 場景 | 建議 | 原因 |
|------|------|------|
| 查詢 Spring Boot 2.x 的 `@Autowired` 用法 | 不需查 | 這是穩定 API，AI 訓練資料足夠 |
| 查詢 Spring Boot 3.3 昨天剛發布的新功能 | 需要查 | 訓練資料截止日期可能早於發布日 |
| 查詢一個小眾 Library（< 1000 GitHub stars） | 需要查 | AI 對小眾 Library 的知識可能不完整 |
| 查詢內部 API 文件（公司 Wiki） | 視情況 | 若 Claude Code 可存取內部網路，可考慮；否則手動提供文件 |
| 除錯一個特定版本的 Bug | 需要查 | 該版本的 Release Note 或 Issue 可能包含關鍵資訊 |

## 6. 不使用 MCP 的替代方案

若您的 Claude Code 環境未設定網頁爬取 MCP，仍可透過以下方式取得最新文件：

**方案 A：手動提供文件內容**
```bash
# 使用 curl 下載文件並儲存為 Markdown
curl -s https://example.com/api-docs | pandoc -f html -t markdown -o api-docs.md

# 在 Claude Code 中用 @ 參照
> 請根據 @api-docs.md 的最新內容，修正 TicketController 的 API 實作
```

**方案 B：讓 Claude 讀取本地已下載的文件**
```bash
# 使用 wget 遞迴下載文件網站
wget -r -l 2 -np -k https://docs.example.com/

# 在 Claude Code 中引用
> @docs.example.com/ 中的最新 API 規範是什麼？
```

**方案 C：直接貼上關鍵段落**
```
Spring Security 6.3 的官方文件中有以下更新：
[貼上從瀏覽器複製的官方文件內容]

請根據這些更新，調整我們的 AuthController 實作。
```

## 7. 小結

1. AI 的訓練資料有截止日期，對最新 API 或小眾 Library 需要額外的文件查詢機制
2. 網頁爬取工具是 Claude Code 的「即時查詢能力」，解決知識時效性問題
3. 判斷何時該查最新文件是重要的成本意識——不是每次都需要爬取
4. 即使沒有 MCP 工具，手動提供文件（curl + pandoc）也能達到相同效果

## 8. 延伸練習

1. 查詢一個你常用的 Library 最新版本的新功能，對比 Claude 的既有知識與官方文件的差異
2. 嘗試用 curl + pandoc 手動下載一份線上文件，在 Claude Code 中用 `@` 參照引用
3. 建立一份團隊的「需要查最新文件的 Library 清單」，附上官方文件 URL

## 9. 查核來源與版本備註

- 來源：Anthropic MCP Server 文件、網頁爬取最佳實務
- 查核日期：2026-06-06（已核實）
- 版本備註：MCP Server 的套件名稱與設定方式以 Claude Code 最新官方文件為準

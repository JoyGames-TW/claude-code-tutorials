# 03-2-3 前端優化類：React 元件品質與 Web 效能分析

> ⚠️ **線上核實狀態**：已核實（2026-06-06）。React 效能最佳化技巧（useMemo、虛擬化、Code Splitting）與 Core Web Vitals 指標皆正確。
> **注意**：「reactcomponents」與「web-perf」作為 Claude Code 內建 Skill 的可用性需以實際版本為準。
> 本章的效能分析框架與程式碼範例（優化前後對比）在任何版本中都具有教學價值。

## 1. 本章學習目標

- 理解 Claude Code 中前端輔助 Skill 的定位與使用場景
- 學會使用 reactcomponents Skill 產生符合最佳實務的 React 元件
- 掌握 web-perf Skill 進行前端效能分析與最佳化建議
- 理解如何將這些 Skill 整合進日常前端開發流程
- 建立「效能與品質從開發階段就內建」的思維

## 2. 適用對象與前置知識

- **適用對象**：React 前端開發者、關注前端效能的全端工程師
- **前置知識**：React 18+ 開發經驗（02-2-1）、基本前端效能概念（LCP、FID、CLS）
- **關聯章節**：前接 [03-2-2 docx/pdf 處理](./03-2-2-docx-pdf-summary-and-report-generation.md)，後接 [03-2-4 claude-api 多代理](./03-2-4-claude-api-agents-sdk-multi-agent.md)

## 3. 核心概念

### 3.1 前端開發中 AI 的典型瓶頸

Claude 產出的前端程式碼常見問題：
1. **元件過大**：單一元件包含過多職責，難以維護
2. **不必要的重新渲染**：缺少 `useMemo`、`useCallback`、`React.memo`
3. **效能陷阱**：大量資料未虛擬化、圖片未延遲載入
4. **無障礙缺失**：缺少 aria 屬性、語意化標籤

### 3.2 reactcomponents Skill

這個 Skill 幫助 Claude 產出符合 React 最佳實務的元件，包括：
- 適當的元件拆分（Presentational vs Container）
- 正確的 Hooks 使用（避免常見的 Hooks 陷阱）
- TypeScript 型別完整性
- 無障礙屬性

### 3.3 web-perf Skill

這個 Skill 幫助 Claude 分析前端效能問題，包括：
- React 渲染效能（不必要的 re-render）
- 資源載入策略（Code Splitting、Lazy Loading）
- Core Web Vitals（LCP、FID/INP、CLS）
- Bundle 大小分析

## 4. 操作步驟

### 4.1 使用 reactcomponents Skill 優化元件

```
請使用 reactcomponents Skill 檢查 @frontend/src/components/TicketTable.tsx。

分析以下維度：
1. 元件拆分是否合理（單一職責原則）
2. Hooks 使用是否正確（依賴陣列、避免條件式呼叫）
3. 是否有不必要的重新渲染
4. 無障礙屬性是否完整
5. TypeScript 型別是否安全（避免 any）

並提供重構建議與修正程式碼。
```

### 4.2 使用 web-perf Skill 分析效能

```
請使用 web-perf Skill 分析 @frontend/src/ 的前端程式碼。

重點分析：
1. 是否有大型的同步渲染（阻塞主執行緒）
2. 列表渲染是否使用虛擬化（windowing）
3. 圖片與字型載入策略
4. Code Splitting 是否適當
5. Bundle 中是否有重複的依賴

提供優先級排序的最佳化建議。
```

### 4.3 常見的效能最佳化方案

```typescript
// ❌ 每次渲染都建立新的物件（導致不必要的 re-render）
function TicketList({ tickets }: Props) {
  const sorted = tickets.sort((a, b) => ...); // mutation + 每次都是新 reference
  return <div>{sorted.map(...)}</div>;
}

// ✅ 使用 useMemo 記憶排序結果
function TicketList({ tickets }: Props) {
  const sorted = useMemo(() => 
    [...tickets].sort((a, b) => ...), 
    [tickets]
  );
  return <div>{sorted.map(...)}</div>;
}

// ❌ 大型列表直接渲染
function TicketTable({ tickets }: Props) {
  return <div>{tickets.map(t => <TicketRow key={t.id} {...t} />)}</div>;
}

// ✅ 使用虛擬化（react-window 或 @tanstack/virtual）
function TicketTable({ tickets }: Props) {
  const rowVirtualizer = useVirtualizer({ count: tickets.length, ... });
  return <div>{rowVirtualizer.getVirtualItems().map(...)}</div>;
}
```

## 5. 常見錯誤與排查方式

### 錯誤 1：忽略 React 的渲染機制

**原因**：不熟悉 React 的 reconciliation 機制，未使用 `useMemo`/`useCallback`。

**症狀**：頁面互動卡頓，React DevTools Profiler 顯示大量不必要的 re-render。

**修正**：使用 React DevTools Profiler 定位問題元件，加上適當的記憶化。

### 錯誤 2：未設定 Key 或使用 Index 作為 Key

**原因**：圖方便使用 `map((item, index) => <div key={index}>`。

**症狀**：列表重新排序時出現 UI 異常（動畫錯亂、輸入框內容錯位）。

**修正**：使用穩定且唯一的 ID 作為 Key。

### 錯誤 3：在元件內定義元件

**原因**：在父元件內定義子元件函數。

**症狀**：每次父元件渲染，子元件都被視為全新的元件型別，導致完全重新 Mount（而非 Update）。

**修正**：將子元件定義在父元件外部，或使用 `useCallback` 穩定 reference。

### 錯誤 4：未處理 Suspense 與 Error Boundary

**原因**：只關注「正常路徑」，未考慮載入與錯誤狀態。

**症狀**：Lazy Loading 的元件載入失敗時整個頁面崩潰。

**修正**：使用 `<Suspense>` 包裝 Lazy 元件，使用 Error Boundary 捕獲錯誤。

## 6. 最佳實務

1. **在 CLAUDE.md 中定義前端效能基線**：如「列表超過 50 項必須使用虛擬化」、「所有圖片必須使用 lazy loading」
2. **先讓 Claude 產出，再用 Skill 檢查**：開發與檢查分離。先用 Claude 快速產出 UI，再用 Skill 進行品質把關
3. **效能檢查自動化**：將 web-perf Skill 的檢查整合到 PR 前的 Hook 中（類似 03-1-3 的安全檢查）
4. **使用 React DevTools 驗證 Claude 的建議**：不要盲目套用 Claude 的效能建議。先在 Profiler 中確認瓶頸
5. **效能與可讀性的平衡**：不是所有 `useMemo` 都是必要的。簡單的計算不需要記憶化，過度優化反而降低可讀性
6. **Core Web Vitals 是北極星指標**：LCP < 2.5s、INP < 200ms、CLS < 0.1。讓 Claude 的效能建議都對齊這些指標
7. **定期審查 Bundle 大小**：使用 `vite-bundle-visualizer` 或 `source-map-explorer`，讓 Claude 分析並建議最佳化

## 7. 安全性與成本注意事項

### 安全性
- 前端效能最佳化不應犧牲安全性。例如，不要為了效能而省略輸入驗證
- 第三方 Library 的選擇應考慮供應鏈安全（檢查維護狀態、已知漏洞）

### 成本
- reactcomponents 和 web-perf Skill 的執行約消耗 2,000-5,000 Token
- 效能最佳化是投資——前期花時間優化，後期節省使用者等待時間與伺服器資源

## 8. 小結

1. reactcomponents Skill 確保 Claude 產出的 React 元件符合最佳實務（拆分、Hooks、無障礙）
2. web-perf Skill 分析前端效能瓶頸，提供對齊 Core Web Vitals 的最佳化建議
3. 常見前端效能問題：不必要 re-render、未虛擬化大列表、資源載入策略不佳
4. 效能建議需要人工判斷——不是所有 `useMemo` 都是必要的
5. 將品質與效能檢查納入 Claude Code 的日常開發流程（Prompt + Skill + Hook）

## 9. 延伸練習

### 練習一：元件優化實作
1. 選擇一個 Claude 產出的 React 元件
2. 使用 reactcomponents Skill 分析其品質
3. 根據建議重構元件
4. 使用 React DevTools Profiler 對比重構前後的渲染次數

### 練習二：團隊前端品質標準設計
為團隊設計一套「React 程式碼品質檢查清單」，包含：
- 元件設計規範（至少 5 條）
- 效能規範（至少 5 條）
- 無障礙規範（至少 3 條）
- 如何將這些規範轉化為 Claude Code 的 Prompt 或 Skill

## 10. 查核來源與版本備註

- 來源：React 官方文件、Web Vitals 官方文件、Anthropic Claude Code Skill 文件
- 查核日期：2026-06-06（已核實）
- 版本備註：Skill 名稱與功能以 Claude Code 最新版本為準；本章的效能分析框架與程式碼範例具有通用教學價值

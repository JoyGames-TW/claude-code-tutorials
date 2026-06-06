# 01 Git、GitHub 與 Claude Code 起手式：正確的開發治理與環境建置

> ✅ **全部教材已於 2026-06-06 完成線上核實**。每份教材文件首行皆有 ⚠️ 核實狀態標記，標註了內容的準確度與需要注意的版本差異。

本單元旨在建立正確的 AI 開發起手式。學員將理解 Claude Code 如何強化版本控制與專案治理，而非取代開發紀律。內容涵蓋環境安裝、各類訂閱方案的 API 成本精算、模型選擇策略，以及 VS Code 插件與 CLI 模式的權限切換實務。

> 📚 **教材文件目錄**：[curriculum/README.md](./curriculum/README.md)

## 01-1 環境安裝、登入與操作模式

- **01-1-1** [Windows PowerShell 7+ 安裝與 claude doctor 健檢](./curriculum/01-1-1-windows-powershell-claude-doctor.md)
- **01-1-2** [基本指令操作：`/login`、`@` 參照、`/help`、`/init`](./curriculum/01-1-2-basic-commands-login-reference-help-init.md)
- **01-1-3** [訂閱方案與 API 成本精算（Pro/Team/Enterprise）](./curriculum/01-1-3-subscription-plans-api-cost-estimation.md)
- **01-1-4** [額度重置規則與 Extra Usage 延伸計費說明](./curriculum/01-1-4-usage-limits-extra-usage-billing.md)

## 01-2 可用模型選擇與權限模式切換

- **01-2-1** [模型別名與場景：Sonnet 4.6、Opus 4.6、Haiku 4.5](./curriculum/01-2-1-model-aliases-and-use-cases.md)
- **01-2-2** [推理深度控制：`/effort` 指令與 ultrathink 觸發](./curriculum/01-2-2-effort-ultrathink-reasoning-control.md)
- **01-2-3** [五種操作模式：逐步確認、自動編輯、規劃模式、自動模式、全開模式](./curriculum/01-2-3-permission-and-operation-modes.md)
- **01-2-4** [VS Code 插件與 CLI 終端機模式的感知差異](./curriculum/01-2-4-vscode-extension-vs-cli-mode.md)

## 01-3 Git 常用操作與 GitHub CLI 協作流

- **01-3-1** [讓 AI 協助撰寫 Commit Message、整理 Diff 與決定 Staging 範圍](./curriculum/01-3-1-ai-assisted-commit-diff-staging.md)
- **01-3-2** [gh CLI 工作流：一鍵建立 Repo、查看 Issue 與發出 PR](./curriculum/01-3-2-github-cli-repo-issue-pr-workflow.md)
- **01-3-3** [整合示範：開發完畢後自動生成 PR Title 與 Body](./curriculum/01-3-3-auto-generate-pr-title-and-body.md)

## 01-4 SDD 文件驅動開發與主專案設定

- **01-4-1** [SDD（Specification-Driven Development）概念：規格即契約](./curriculum/01-4-1-specification-driven-development.md)
- **01-4-2** [產出 spec.md：定義資料模型、API 端點與頁面行為](./curriculum/01-4-2-create-spec-md-data-api-ui-behavior.md)
- **01-4-3** [CLAUDE.md 進階設定：宣告 MCP Server 與 Hooks](./curriculum/01-4-3-claude-md-mcp-server-hooks.md)
- **01-4-4** [主專案情境：AI 問題追蹤系統（Ticket/User/Comment）架構](./curriculum/01-4-4-ai-ticket-system-architecture.md)

---

# 02 全端主線實作：後端生成、TDD 測試驅動與前端驗證

本章節展示 Claude Code 如何參與設計、撰寫、測試與整合的完整生命週期。學員將學習如何運用測試驅動開發（TDD）確保後端邏輯正確，並透過 Playwright MCP 讓 Claude 具備自動化操作瀏覽器、驗證前端畫面結構的能力。

## 02-1 後端生成、TDD 先行與自主修正循環

- **02-1-1** [TDD 概念：Red、Green、Refactor 三循環實務](./curriculum/02-1-1-tdd-red-green-refactor.md)
- **02-1-2** [自動化開發：讓 Auto Mode 自主跑完測試與修正迴圈](./curriculum/02-1-2-auto-mode-test-and-fix-loop.md)
- **02-1-3** [Spring Boot 3 骨架生成：Entity、Service、Controller 與 DTO](./curriculum/02-1-3-spring-boot-entity-service-controller-dto.md)

## 02-2 前端框架、API 串接與 Playwright 畫面驗證

- **02-2-1** [React + Vite 快速落 UI：搭配 spec.md 確保規格一致性](./curriculum/02-2-1-react-vite-ui-with-spec-md.md)
- **02-2-2** [前後端整合：透過 `@` 參照控制器實現精準 API 串接](./curriculum/02-2-2-api-integration-with-controller-reference.md)
- **02-2-3** [Playwright MCP：利用 browser_snapshot 進行無障礙樹結構分析](./curriculum/02-2-3-playwright-mcp-browser-snapshot.md)
- **02-2-4** [自動化測試：讓 Claude 撰寫 E2E 測試腳本並納入 CI 管道](./curriculum/02-2-4-e2e-tests-and-ci-pipeline.md)

## 02-3 真實除錯與上下文管理

- **02-3-1** [常見錯誤排查：CORS、分頁格式與資料綁定誤區](./curriculum/02-3-1-debugging-cors-pagination-binding.md)
- **02-3-2** [`/bug` 指令：切出專門除錯上下文，聚焦問題核心](./curriculum/02-3-2-bug-command-debug-context.md)
- **02-3-3** [`/rewind` 與 `/compact`：探索回退與壓縮長對話雜訊](./curriculum/02-3-3-rewind-compact-context-management.md)

---

# 03 企業級 Skill 製作、輔助技能與背景 Agent 應用

目標是讓學員將開發規則與長任務轉化為可重用的工作流。本單元將教授如何製作企業資安規範檢查 Skill，並介紹各類開發輔助工具。此外，將深入解析 `/agent` 如何在背景處理高噪音任務，實現多代理平行協作。

## 03-1 用 skill-creator 製作企業資安規範檢查 Skill

- **03-1-1** [企業情境：禁止硬編碼、外部請求檢查與權限驗證規範](./curriculum/03-1-1-enterprise-security-policy-scenarios.md)
- **03-1-2** [技能拆解：定義輸入、檢查清單與建議修正模板](./curriculum/03-1-2-skill-input-checklist-fix-template.md)
- **03-1-3** [Hooks 應用：在寫入檔案前自動觸發資安檢查](./curriculum/03-1-3-hooks-before-file-write-security-check.md)

## 03-2 開發輔助技能分類導覽與應用

- **03-2-1** [外部資料補充：firecrawl 爬取最新文件與 API](./curriculum/03-2-1-firecrawl-docs-api-research.md)
- **03-2-2** [文件處理類：docx/pdf 規格書摘要與結案報告產出](./curriculum/03-2-2-docx-pdf-summary-and-report-generation.md)
- **03-2-3** [前端優化類：reactcomponents 與 web-perf 效能分析](./curriculum/03-2-3-reactcomponents-web-performance-analysis.md)
- **03-2-4** [AI 整合開發：claude-api 與 agents-sdk 多代理應用](./curriculum/03-2-4-claude-api-agents-sdk-multi-agent.md)

## 03-3 深入講解 /agent 背景長任務

- **03-3-1** [適合交給 Agent 的工作：假資料生成、資料清理、Log 分析](./curriculum/03-3-1-agent-tasks-fake-data-cleaning-log-analysis.md)
- **03-3-2** [定義任務邊界：不干擾主線的背景工作法](./curriculum/03-3-2-agent-task-boundary.md)
- **03-3-3** [Git Worktrees 與平行 Session：一個修 Bug，一個開發新功能](./curriculum/03-3-3-git-worktrees-parallel-sessions.md)

---

# 04 Review、程式優化與收尾

本單元建立寫完程式後的自我審查流程。透過內建的 `/review` 與 `/simplify` 指令，讓 Claude Code 從單純的生成器轉變為長期的工程夥伴。最後將回顧整體 SDD、TDD 到 CI/CD 的完整開發命週期。

## 04-1 /review 與 /simplify 內建品質指令

- **04-1-1** [程式碼審查：正確性、安全性、可讀性三維度評核](./curriculum/04-1-1-code-review-correctness-security-readability.md)
- **04-1-2** [`/simplify` 重構精簡：消除過度設計與重複代碼](./curriculum/04-1-2-simplify-refactoring.md)
- **04-1-3** [收尾循環：`/review` 搭配 Follow-up Prompt 追問修正細節](./curriculum/04-1-3-review-follow-up-prompt-loop.md)

## 04-2 Slash Commands 完整工作流總覽

- **04-2-1** [常用指令開發時序表：從 `/init` 到 `/clear` 的應用時機](./curriculum/04-2-1-slash-command-development-timeline.md)
- **04-2-2** [自動化 PR 交付：整理 Diff、生成說明並發布 PR](./curriculum/04-2-2-automated-pr-delivery.md)
- **04-2-3** [課程總結：建立穩定開發工作流的核心心法回顧](./curriculum/04-2-3-stable-development-workflow-summary.md)

---

> 📚 **完整教材索引**：[curriculum/README.md](./curriculum/README.md)
> 
> 所有章節皆有獨立 `.md` 教材文件，位於 `./curriculum/` 目錄下。
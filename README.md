# Local RAG Customer Service

純前端、可直接部署到 GitHub Pages 的本地 RAG 客服 MVP。

## 特點

- 純 HTML / CSS / JavaScript
- 可部署到 GitHub Pages
- 使用者自行輸入 API Key
- API Key 預設不保存
- 可選擇僅保存到 `sessionStorage`
- 支援 `.txt` / `.md`
- 不做 Chunking
- 不做 Embedding
- 不需要 Vector Database
- 使用整份文件做 BM25 Retrieval
- Query Rewrite（使用 API Key；未設定或失敗時沿用原始 Query）
- 可在網頁維護同義詞組，保存於瀏覽器 `localStorage`
- 無文件達最低 BM25 分數時，自動 fallback 到 All Context
- 支援手動 All Context 模式
- 支援 Debug Retrieval
- 支援 OpenAI-compatible API

## 本地測試

直接用瀏覽器開啟 `index.html` 即可。

部分 LLM Provider 可能因 CORS 政策不允許瀏覽器直接呼叫，
此時需要換成允許 browser request 的 provider，或未來加入自己的 backend proxy。

## UI Smoke Test

```sh
python3 -m http.server 8000
```

開啟 `http://localhost:8000/tests/ui-smoke.html`。

## GitHub Pages 部署

1. 建立 GitHub repository
2. 將本專案內容 push 到 repository
3. 前往 `Settings -> Pages`
4. Source 選擇 `Deploy from a branch`
5. Branch 選擇 `main`
6. Folder 選擇 `/ (root)`
7. 儲存

## API Key

本專案不內建 API Key。

使用者必須自行輸入：

- API Base URL
- Model
- API Key（只在 OpenAI-compatible API 模式需要）

Query Rewrite 會使用目前設定的 API 呼叫一次 LLM；沒有 Key 或改寫失敗時改用原始問題。Synonym Expansion、BM25 與低分數 fallback 都在瀏覽器本機執行，不需要 API Key。使用 API 模式產生最終回答仍需要 API Key。

預設 API Key 不會持久保存。

如果勾選 Session 保存，只會寫入 `sessionStorage`，關閉分頁後即失效。

## 架構

```text
Local TXT / Markdown
        ↓
Browser
        ↓
Optional LLM Query Rewrite (API Key required)
        ↓
Local synonym expansion (no API Key)
        ↓
BM25 whole-document retrieval (no API Key)
        ↓
Top K documents, or All Context if no document reaches Min Score (no API Key)
        ↓
Prompt Builder
        ↓
User-provided API Key
        ↓
LLM API
        ↓
Answer + Sources
```

## Security

因為這是純 GitHub Pages 靜態版本：

- API Key 一定存在使用者自己的瀏覽器記憶體中
- 不會被寫進 repository
- 不會送到你的 server
- 但瀏覽器 Extension、惡意 script 或 XSS 理論上仍可能讀到 Key

建議使用獨立測試 Key，並設定 API 使用額度 / Rate Limit。

## License

MIT

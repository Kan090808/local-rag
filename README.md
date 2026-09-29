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
- 支援 All Context 模式
- 支援 Debug Retrieval
- 支援 OpenAI-compatible API

## 本地測試

直接用瀏覽器開啟 `index.html` 即可。

部分 LLM Provider 可能因 CORS 政策不允許瀏覽器直接呼叫，
此時需要換成允許 browser request 的 provider，或未來加入自己的 backend proxy。

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
- API Key

預設 API Key 不會持久保存。

如果勾選 Session 保存，只會寫入 `sessionStorage`，關閉分頁後即失效。

## 架構

```text
Local TXT / Markdown
        ↓
Browser
        ↓
BM25 whole-document retrieval
        ↓
Top K documents
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

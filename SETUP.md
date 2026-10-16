# Stock Base｜股票研究總站

繁體中文、適合手機及電腦的股票研究資料庫。GitHub Pages 提供讀者頁面，管理員從網站選擇 HTML 報告並透過 GitHub API 寫入同一 repository。訪客重新整理後看到新部署內容。

## 首次發佈（一次設定）

1. 在 GitHub 建立專用公開 repository，例如 `stock-base`，以 README 初始化 `main` 分支。
2. 把本套件解壓，將 **index.html**、**data/reports.json** 放到 repository 根目錄及 `data` 資料夾。可一併上載 README、`.nojekyll`、範本；不要把整個 ZIP 當作網站上載，也不要多加一層 stock-base 目錄。
3. Repository → Settings → Pages → Source 選 **Deploy from a branch** → `main` → `/(root)` → Save。
4. 等待 GitHub Pages 部署成功。使用 Settings → Pages 顯示的網址；一般為 `https://你的帳號.github.io/stock-base/`。
5. 網站右上「報告管理」→「GitHub 連線設定」，填寫帳號、repository 及 `main`。
6. 在 GitHub Settings → Developer settings → Personal access tokens → Fine-grained tokens 建立 token，只選這個 repository，Repository permissions 的 **Contents: Read and write**，設定合理到期日。貼入管理頁後連線。不要將 token 寄給其他人或寫入報告。

Pages 設定及 token 權限由 GitHub 帳戶持有人操作。本套件沒有內建憑證，也不會自動建立 repository。

## 每次新增報告

1. 按「報告管理」，連線 GitHub。
2. 選擇或拖放一份 `.html`／`.htm`（每份不超過 2 MB）。
3. 核對公司、代號、市場、研究日期、標題及摘要，選擇分析角度，填寫標籤。
4. 按「預覽報告」，確認內容。
5. 確認適合公開，再按「發佈到 GitHub」。
6. 成功提交不等於 Pages 已更新；在 repository 的 Actions 查看部署結果。部署成功後訪客按「更新」或重新整理。

Token 只在頁面記憶體中使用；關閉管理頁或重新整理即清除。只有非敏感連線設定（帳號、repository、分支）存到 localStorage。草稿沒有持久儲存，離開頁面前請保留原始 HTML。

## 報告格式相容

對應股票 skill 的八部分：投資結論速覽、公司與行業、財務表現與質素、估值與情景、股息與收息價值、短線走勢與事件、主要風險與追蹤清單、資料來源與限制。

現有 HTML 不必重寫，但首頁摘要必須核對。報告可在 `<head>` 加入以下 meta 標記以自動帶入欄位，見 `report-template.html`：

```html
<meta name="stock-name" content="公司名稱">
<meta name="stock-ticker" content="0388.HK">
<meta name="stock-market" content="HK">
<meta name="report-date" content="2026-10-16">
<meta name="report-title" content="報告標題">
<meta name="description" content="核心結論摘要">
<meta name="keywords" content="金融,交易所">
```

市場：HK / US / CN / OTHER。請在每次研究按實際資料日期填寫，並統一代號格式（例如不要混用 388.HK 與 0388.HK）。檔名及標題的自動猜測不會取代人工核對。

報告在隔離 iframe 中呈現。CSS 內嵌樣式、HTML 表格、內嵌 SVG、data URI 圖片可呈現；JavaScript、表單、外部字型／樣式／圖片與 iframe 不執行。Chart.js 等動態圖表請在原報告先轉成內嵌 SVG 或 PNG。HTTP(S) 來源連結可另開視窗。未將原始檔案放在可直接執行的獨立 HTML URL。

## 版本、分享及備份

- 同市場、同代號、不同研究日期：保留不同版本。
- 同市場、同代號、同日期：發佈前要求確認替換，舊內容仍可從 Git 歷史恢復。
- 首頁預設每隻股票最新一份，可取消勾選以列出全部版本。
- 閱讀報告後按「複製報告連結」，可分享固定 ID 連結。下架後該連結失效。
- 「已發佈報告與備份」可匯出完整 `reports.json`。復原時在 GitHub 將備份上載至 `data/reports.json` 覆蓋，提交後等候部署。
- 下架只移除目前清單，不會抹除 Git 歷史。
- 發佈先讀最新遠端內容，再附 SHA 提交；如有人同時修改導致 409，操作會停止，重新連線再試。

## 技術與容量

無 npm、無外部 JavaScript 依賴、無 build 步驟。`index.html` 包含頁面與管理程式。`data/reports.json` 包含報告 metadata 及原 HTML，單次提交維持清單與內容一致。

為避免讀者一次下載過大的資料庫，管理程式限制資料庫 20 MB；報告多時宜另建年度 archive repository，未來可升級成分檔索引。這一版不提供自動行情、投資評級計算、批次匯入或自動更新研究內容。

公開 Pages 網站及公開 repository 的內容可被任何人讀取，也可能被搜尋引擎收錄，並非「只知連結才有權限」。不要上載私人持倉、銀行客戶資料、個人資料或憑證。

## 驗證狀態

已進行 JavaScript 語法與資料處理測試（繁中編碼、最新版本分組、格式驗證及 GitHub 衝突處理）。尚未進行真實 repository 寫入／Pages 部署及瀏覽器視覺驗證；需部署至指定 repository 後確認。

## 官方文件

- GitHub Pages 發佈設定：https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site
- GitHub Contents API：https://docs.github.com/en/rest/repos/contents

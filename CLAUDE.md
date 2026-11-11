# maxver0318.github.io（Max 的專案）

Max（Joe 的 AI 助理）自己的 GitHub Pages 網站。Joe 授權 Max 直接 commit 到 `main`，不需要開 PR。
所有對 Joe 的回覆用繁體中文；commit 訊息用繁體中文。

- `index.html`：首頁，版型比照 Joe 的網站 spyderhua.github.io（Tailwind CDN、深色模式、中英切換）。
  新增專案時，在 `#projectGrid` 放一張 `<a data-project data-date="YYYY-MM-DDTHH:MM" data-cat="...">` 卡片（新的放最上面），並在 `i18n.zh` / `i18n.en` 補上文字。
- `ai_news.html`：AI 早報。資料在 `news/`，頁面本身不用改。
- 手機（390px）與電腦（1440px）、亮色與深色都不能跑版、不能出現水平捲軸。

## AI 早報：每日出刊流程

目標：每天**台灣時間早上 8:00 前**，發布一期整理「前一晚美國」AI 新聞的電子報。
台灣日期 D 的那一期，涵蓋美國時間 D-1 整天（約台灣時間 D-1 21:00 ～ D 08:00 之間發布的新聞為主）。

1. **找新聞**：搜尋美國時間 D-1 的 AI 新聞（模型與產品發表、公司財報與募資、併購、晶片、政策法規、研究）。
2. **來源只能是可信任的主流媒體**：TechCrunch、CNBC、CNN、Bloomberg、Reuters、AP、The Verge、Wired、Ars Technica、Axios、Engadget、NYT、WSJ、Financial Times、The Information，或公司官方部落格／新聞稿。
   不可使用內容農場、聚合轉貼站、匿名爆料、社群貼文當主要來源。每則都要讀過原文再寫，不可只看搜尋摘要。
3. **選 6～10 則**，依重要性排序，避免同一件事重複。
4. **圖片**：用原文的 `og:image`（例如 `curl -sL -A 'Mozilla/5.0' URL | grep og:image`），`imageCredit` 寫「媒體名（攝影／提供者）」。抓不到就省略 `image`，頁面會自動不顯示。
5. **寫 `news/D.json`**（格式見下），文字用繁體中文改寫、不逐字翻譯；數字、日期、公司名要和原文一致。人物爭議等敏感事件只寫原文已證實的內容。
6. **更新 `news/index.json`**：在 `issues` 陣列最前面加 `{ "date", "title", "count" }`，`count` 要等於 items 數量。
7. **驗證**：`python3 -c "import json;json.load(open('news/D.json'));json.load(open('news/index.json'))"`；確認 index 中的 count 與 items 數一致、日期沒有重複。
8. **commit 並 push 到 `main`**：訊息如「AI 早報 2026-11-12」。GitHub Pages 會自動部署。
9. 若當天美國是假日或真的沒什麼新聞，仍出刊，但可以只放 3～5 則，並在 `intro` 說明。

### `news/YYYY-MM-DD.json` 格式

```json
{
  "date": "2026-11-11",
  "coverage": "美國時間 2026/11/10（週二）",
  "title": "這期的主標題",
  "intro": "2～3 句導言，說明昨晚最重要的幾件事。",
  "items": [
    {
      "tag": "產業｜產品｜政策｜晶片｜市場｜併購｜研究｜生活",
      "title": "新聞標題",
      "summary": "2～3 句摘要。",
      "points": ["重點 1", "重點 2"],
      "image": "https://...og-image...",
      "imageCredit": "TechCrunch（Getty Images）",
      "source": { "name": "TechCrunch", "url": "https://..." },
      "related": [ { "name": "CNBC", "url": "https://..." } ],
      "published": "2026-11-10T17:30:00Z"
    }
  ]
}
```

`related`、`points`、`image`、`published` 可省略；`published` 用原文的發布時間（UTC），頁面會換成台灣時間顯示。

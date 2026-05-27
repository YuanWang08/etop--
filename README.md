# 智慧計算學門發展趨勢暨學門交流會

活動官方網站（單頁靜態站）。

- **活動日期**：115 年 05 月 08 日（五）08:30 – 17:00
- **活動地點**：台中・清新溫泉飯店
- **主辦單位**：國科會智慧計算學門
- **協辦單位**：台灣 AI 卓越中心（Taiwan AICoE）
- **指導單位**：國家科學及技術委員會

## 技術概覽

純靜態 HTML / CSS / JavaScript，無建置流程、無依賴套件，所有內容集中在 `index.html`（含內嵌的 `<style>` 與 `<script>`）。透過 GitHub Pages 部署。

## 專案結構

```
.
├── index.html                # 全站唯一頁面（含樣式與互動邏輯）
├── images/
│   ├── poster.jpg            # 活動海報
│   ├── qrcode.svg            # Keynote 直播 QR code
│   ├── Katia.png             # 講者照片
│   ├── Mike.jpg              # 講者照片
│   └── 活動照片/
│       ├── 上午/             # morning_01.jpg ~ morning_26.jpg
│       └── 下午/             # afternoon_01.jpg ~ afternoon_21.jpg
└── .github/workflows/
    └── deploy-pages.yml      # 推到 main/master 自動部署到 GitHub Pages
```

## 本地預覽

不需要建置工具，但因為網頁會抓取本地圖片與 YouTube 嵌入，建議用本地伺服器跑：

```bash
# Python（內建即可）
python -m http.server 8000

# 或 Node.js
npx serve .
```

然後開 `http://localhost:8000`。直接點開 `index.html`（file://）部分功能（拜訪計數、YouTube embed）可能會受限。

## 部署

推送到 `main` 或 `master` 後，`.github/workflows/deploy-pages.yml` 會自動部署到 GitHub Pages。手動觸發也可從 Actions 頁面執行 `workflow_dispatch`。

## 內容維護指南

### 更換 YouTube 影片

在 `index.html` 的 `<section id="recap">` 內，三個 `<iframe>` 的 `src` 替換成正式影片網址（embed 格式）：

```
https://www.youtube.com/embed/{影片ID}
```

把 `https://youtu.be/PGXMxroWEh8` 轉成 embed URL 的方法：取 `/` 後面的 ID，貼到上面的 `{影片ID}`。

### 新增 / 替換活動照片

照片放在 `images/活動照片/上午/` 或 `images/活動照片/下午/`，命名規則：

- 上午：`morning_01.jpg` ~ `morning_NN.jpg`（連號、兩位數補零）
- 下午：`afternoon_01.jpg` ~ `afternoon_NN.jpg`

更換完照片後，到 `index.html` 修改以下兩處：

1. **預載縮圖**：每個 `.photo-grid` 內前 6 張 `<img>` 的 `src`、`data-index`（若數量不變可不動）。
2. **總張數**：兩個 `.photo-block` 容器的 `data-total` 屬性、`.photo-block-count` 的「共 N 張」、`.photo-more-label` 的「查看全部 N 張」。
3. **JS 設定**：頁尾 `var galleries = { ... }` 內 `morning.total` / `afternoon.total` 也要對應修改。

剩餘照片由 JS 在使用者點「查看全部」時依命名規則動態載入，所以新增的照片只要照命名規則放好、把 total 加上去即可。

### 修改議程 / 講者 / 座談陣容

直接編輯 `index.html` 對應 `<section>` 的內容：

| 區塊 ID | 內容 |
| --- | --- |
| `#recap` | 活動回顧（影片 + 照片精選） |
| `#poster` | 活動海報 |
| `#speakers` | 專題演講（Keynotes） |
| `#panel-discussion` | 專題座談陣容 |
| `#networking` | 交流活動（世界咖啡廳桌長） |
| `#agenda` | 議程表 |
| `#location` | 地點資訊 |

導覽列順序由 `<nav>` 內 `.nav-links` 的 `<a>` 順序決定，可獨立調整。

## 第三方服務

- **GoatCounter**：以 `https://etop.goatcounter.com/count` 為端點記錄拜訪人次，並在頁尾顯示累計次數。
- **Google Maps**：嵌入清新溫泉飯店位置（地點區塊）。
- **Microsoft Teams**：Keynote 直播會議連結，由首頁右側 QR Code 連出。

## 聯繫

- 活動聯繫：陳小姐｜vicky945@ncu.edu.tw｜03-4227151 #35348

# 小柯攝影工作室｜作品集網站

臺中婚紗、婚禮紀錄、形象照攝影師的單頁作品集網站。純 HTML + CSS（寫在同一個 `index.html`），不需要任何建置工具，可直接部署到 Vercel。

## 專案結構

```
xiaoke-photo/
├── index.html            ← 整個網站（HTML + CSS + 少量 JS）
├── images/               ← 所有圖片
│   ├── hero.jpg / about.jpg
│   ├── wedding-01 ~ 06.jpg     婚紗
│   ├── ceremony-01 ~ 06.jpg    婚禮紀錄
│   └── portrait-01 ~ 06.jpg    形象照
├── download-images.sh    ← 一鍵下載 Unsplash 免費示意圖
├── vercel.json           ← Vercel 設定（快取、乾淨網址）
└── README.md
```

## 第 0 步：換上真正的照片

`images/` 裡目前是**佔位圖**（柔和漸層色塊），讓你一打開就能看到版面。換成照片有兩種方式：

1. **一鍵下載免費圖庫照片**（Mac / Linux / Windows 的 Git Bash）：
   ```bash
   bash download-images.sh
   ```
   圖片來自 [Unsplash](https://unsplash.com/license)，可免費商用、不需標註。若有幾張失敗或主題不合，到 Unsplash 或 [Pexels](https://www.pexels.com/zh-tw/) 搜尋 `wedding couple`、`wedding ceremony`、`portrait natural light` 手動下載並**用相同檔名**覆蓋即可。
2. **換成攝影師自己的作品**（正式上線一定要這樣做）：直接用同檔名覆蓋，建議長邊 1600px、JPG 品質 80，單張控制在 400KB 內。

> 換照片後，記得順手檢查 `index.html` 裡對應的 `alt` 文字是否還描述正確——alt 是 SEO 與視障使用者的重要資訊。

## 第 1 步：上傳到 GitHub

**方法 A：網頁上傳（不用指令）**
1. 登入 GitHub → 右上 `+` → **New repository**，名稱例如 `xiaoke-photo`，選 Public，按 Create。
2. 在新 repo 頁面點 **uploading an existing file**，把 `index.html`、`images` 資料夾、`vercel.json`、`README.md` 一起拖進去 → **Commit changes**。

**方法 B：Git 指令**
```bash
cd xiaoke-photo
git init
git add .
git commit -m "first commit: 小柯攝影作品集"
git branch -M main
git remote add origin https://github.com/你的帳號/xiaoke-photo.git
git push -u origin main
```

## 第 2 步：部署到 Vercel

1. 到 [vercel.com](https://vercel.com) 用 GitHub 帳號登入。
2. **Add New → Project** → 選剛剛的 `xiaoke-photo` → **Import**。
3. Framework Preset 選 **Other**，Build Command 與 Output Directory **都留空** → **Deploy**。
4. 約 30 秒後會得到網址，例如 `https://xiaoke-photo.vercel.app`。

之後每次 push 到 GitHub，Vercel 會自動重新部署。

## 第 3 步：上線後要改的地方

在 `index.html` 搜尋並替換：

| 搜尋 | 改成 |
|---|---|
| `https://xiaoke-photo.vercel.app/` | 你實際的網址（canonical） |
| `hello@xiaoke-photo.tw` | 真實 Email |
| `xiaoke.photo` | 真實 Instagram 帳號 |
| 方案價格、交件天數 | 實際報價 |

### 讓表單真的寄出信件

目前表單只會在頁面上顯示「已收到」，**不會寄信**。最簡單的做法是使用免費的 [Formspree](https://formspree.io)：

1. 註冊後建立表單，拿到網址如 `https://formspree.io/f/abcdwxyz`。
2. 把 `<form id="inquiry-form" action="#" ...>` 的 `action` 改成該網址。
3. 刪掉 `<script>` 中「詢問表單」那一段（或改成 `fetch` 送出），表單就會寄到你的信箱。

## 教學重點：這個網站用了哪些技巧

- **語義化標籤**：`header` / `nav` / `main` / `section` / `article` / `footer`，每個 `section` 用 `aria-labelledby` 對應標題。
- **SEO**：`<title>`、`meta description`、canonical、Open Graph、JSON-LD 結構化資料（ProfessionalService）、每張圖都有描述性 `alt`。
- **效能**：首屏圖 `fetchpriority="high"`，其餘 `loading="lazy"`；圖片設 `width`/`height` 避免版面跳動。
- **RWD**：作品集 `grid-template-columns` — 桌機 3 欄 → `max-width: 1024px` 平板 2 欄 → `max-width: 768px` 手機 1 欄。
- **可及性**：鍵盤焦點樣式、`aria-pressed` 篩選按鈕、`aria-live` 表單訊息、尊重 `prefers-reduced-motion`。

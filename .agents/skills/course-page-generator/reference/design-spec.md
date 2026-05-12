# Design Spec

這份文件定義 `course-page-generator` 的共同視覺語言，適用於：

- `reference/base.html` 長頁課程頁
- `reference/deck-template.html` HTML Deck 簡報

目標不是讓兩種模式版面完全相同，而是讓它們共享同一套品牌語彙、字體、色彩、卡片與圖片規則。

## 1. Brand Direction

整體風格定位：

- 深色、正式、專業
- 公務簡報與課程頁共用的資訊設計語彙
- 視覺重點靠色彩、字級、留白與卡片分層，而不是裝飾性插畫
- 適合政策說明、法規整理、案例教學、決策簡報

關鍵感受：

- `沉穩`：深色背景、低彩度卡片
- `清楚`：高對比文字、明確層級
- `帶張力`：橘金色重點、漸層標題、局部光暈
- `結構化`：資訊以區塊、卡片、流程、比較表呈現

## 2. Color Tokens

### Dark Theme

```text
--bg: #0f1117
--bg-card: #181b25
--bg-card-alt: #1e2130
--text: #e4e4e7
--text-dim: #8b8d9a
--border: #2a2d3a
--accent: #e8845a
--accent-rgb: 232 132 90
--accent2: #e8b878
--accent2-rgb: 232 184 120
--accent3: #e08898
--accent3-rgb: 224 136 152
--code-bg: #12141c
--radius: 12px
```

### Light Theme

僅 `base.html` 目前支援 light theme。若 deck 未來要支援，應沿用同一組 token 名稱：

```text
--bg: #f5f5f7
--bg-card: #ffffff
--bg-card-alt: #f0f0f3
--text: #1a1a2e
--text-dim: #5a5b6a
--border: #d4d4d8
--accent: #c45c28
--accent-rgb: 196 92 40
--accent2: #9a7028
--accent2-rgb: 154 112 40
--accent3: #b04860
--accent3-rgb: 176 72 96
--code-bg: #eaeaef
```

### Usage Rules

- `--bg` 只用於頁面主背景或整張 slide 背景。
- `--bg-card` 用於主要卡片、資訊框、question node、summary card。
- `--bg-card-alt` 用於次層卡片、縮圖容器、hover 狀態、局部分區。
- `--text` 用於主標題與主要文字。
- `--text-dim` 用於 lead、輔助說明、次要資訊。
- `--border` 用於卡片邊框、表格分隔、tag 邊界。
- `--accent` 是主強調色，用於 CTA、重點字、進度條、活躍狀態。
- `--accent2` 是輔助強調色，用於次要高亮、標籤、列表節點。
- `--accent3` 僅用於補充狀態或需要第三種視覺區分時，避免濫用。

## 3. Typography

### Font Stack

- 內文：`"Noto Sans TC", "Microsoft JhengHei", "PingFang TC", sans-serif`
- 標題：`"Noto Serif TC", "Microsoft JhengHei", "PingFang TC", serif`
- 等寬：`Consolas, "Courier New", monospace`

### Typographic Roles

- `Hero / slide H1`：
  - 課程頁：大標題可使用漸層字
  - Deck：投影片 H1 需在遠距閱讀下仍清楚
- `H2 / card title`：
  - 用於次標、question node、action card title
- `Body`：
  - 說明文字、條列文字、表格內容
- `Meta`：
  - kicker、badge、頁碼、section tag、table header

### Scale Guidance

- `H1`：約 52px 至 70px
- `H2`：約 28px 至 34px
- `Card title`：約 20px 至 24px
- `Lead`：約 19px 至 21px
- `Body`：約 16px 至 18px
- `Meta`：約 11px 至 13px

### Rules

- 標題優先使用 serif，內文優先使用 sans-serif。
- 避免全大寫中文標題；英文 meta 才適合使用大寫與字距。
- 漸層字只用在高階標題，不用在段落或卡片小標。

## 4. Layout

### Course Page

- 長頁滾動閱讀
- Hero → 講師區 → 章節內容 → Summary → Footer
- 章節間距要足夠，避免連續卡片區塊黏在一起

### HTML Deck

- 16:9 全螢幕閱讀
- 一頁一問題或一頁一結論
- 同頁不放過多層級，優先單一主訊息 + 1 組輔助結構

### Shared Spacing Rules

- 卡片內距：`22px` 至 `28px`
- 大區塊間距：`34px` 至 `48px`
- Grid gap：`12px`、`18px`、`24px`、`48px` 依密度選用
- 圓角統一使用 `12px`，縮圖或小 badge 可用 `6px` 至 `8px`

## 5. Cards

### Base Card

```text
background: var(--bg-card)
border: 1px solid var(--border)
border-radius: var(--radius)
```

### Card Rules

- 卡片是本系統最重要的資訊容器。
- Hover 僅做輕微邊框強調與上浮，不做誇張位移。
- 卡片內文盡量控制在 3 至 6 行可掃讀範圍。
- 同一 row 的卡片高度應盡可能一致。
- 卡片內若有 badge，badge 要先於標題出現。

### Card Variants

- `card`：長頁標準卡片
- `type-card`：概覽卡片，可點擊
- `choice-card`：決策選項卡片
- `action-card`：後續作為或措施卡片
- `summary-card`：總結卡片
- `question-node`：決策樞紐或個案導入框

## 6. Tags, Kicker, Badge

### Kicker

- 用於標示內容類型，例如 `QUESTION`、`DETAIL`、`WORKFLOW`
- 風格：
  - 膠囊形
  - 細邊框
  - 小圓點裝飾
  - 弱 accent 背景

### Badge

- 用於卡片前置分類
- 以等寬字體呈現
- 使用 accent / accent2 / accent3 做分類差異
- 不可過大，不可搶過標題

### Rules

- `kicker` 屬於頁級導航語言
- `badge` 屬於卡片級分類語言
- 同一區塊內不要同時堆太多 `kicker` 與 `badge`

## 7. Buttons And Links

### Course Page Buttons

- Hero 導覽按鈕分為 primary / secondary
- Primary：
  - `background: var(--accent)`
  - `color: #fff`
- Secondary：
  - 透明或低對比底
  - `border: 1px solid var(--border)`

### Deck Interaction

- 投影片內互動元件以卡片、表格欄位、選項區塊為主
- 不強制使用傳統 button 外觀
- 點擊區要明確，hover 要有邊框或位移回饋

### Link Rules

- 連結文字使用 accent
- hover 可轉 accent2 或加底線
- 避免大片正文都用彩色連結，僅保留必要導航與外部引用

## 8. Tables And Data Visuals

### Tables

- 表頭用等寬字體與 accent 色
- row 間用 `var(--border)` 分隔
- hover row 可有極弱 accent 背景

### Data Visual Rules

- 只有來源有真實量化資料時才用圖表
- 若無量化資料，改用：
  - action cards
  - comparison matrix
  - process flow
  - decision cards

### Chart Color Priority

- 第一資料序列：`--accent`
- 第二資料序列：`--accent2`
- 第三資料序列：`--accent3`
- 背景網格與輔助線使用低透明灰，不可搶眼

## 9. Images

### General Rules

- 圖片必須服務內容，不是裝飾性填充。
- 優先使用真實照片、現場圖、截圖、流程圖、法規示意。
- 避免模糊、純氣氛、無資訊價值的 stock-like 視覺。

### Image Containers

- 使用固定比例：
  - `16 / 10` 適合卡片與圖文並排
  - Hero / cover 可依模板需求調整
- 圖框需有：
  - 圓角
  - 細邊框
  - 深色背景墊底

### Visual Prompt Rule

- `[visual-prompt]` 是中繼指令，不是前端內容。
- build 前必須轉成實際圖片或移除。
- 視覺 prompt 描述的是「實施後場景」或「具體現場」，不是抽象風格詞堆砌。

## 10. Motion

- 動畫只作為導引，不作為表演。
- 課程頁可接受輕微 fade / reveal。
- Deck 可接受 slide 切換、hover 上浮、進度條更新。
- 避免持續閃爍、快速縮放、過度彈跳。

## 11. Content Density

### Course Page

- 可容納較完整脈絡與細節
- 適合放：
  - lead
  - card grid
  - insight
  - flow
  - summary

### Deck

- 每頁只放一個主訊息
- 文字能卡片化就不要變成長段落
- 一頁不應同時塞入：
  - 大圖表
  - 多段正文
  - 多層決策樹

## 12. Accessibility

- `--text` 與 `--bg` 必須維持高對比
- `--text-dim` 只能用於次要資訊，不可用於關鍵結論
- 點擊目標要足夠大
- 圖片需要有可讀的 `alt`
- 表格與互動元件不能只靠顏色區分

## 13. Anti-Patterns

避免以下風格偏移：

- 純白企業 SaaS 風
- 紫色主導配色
- 過度行銷式 hero
- 過重玻璃擬態
- 裝飾性大漸層 blob
- 太多陰影導致畫面髒
- 太多字級混用
- 一頁塞滿無法掃讀的長段落

## 14. Source Of Truth

這份 spec 對應的主要實作來源：

- [base.html](C:/Users/bbcx1/agent-skill-lecture-builder/.agents/skills/course-page-generator/reference/base.html:42)
- [deck-template.html](C:/Users/bbcx1/agent-skill-lecture-builder/.agents/skills/course-page-generator/reference/deck-template.html:8)

若未來調整 token、字體、卡片語彙，應同步更新這份文件。

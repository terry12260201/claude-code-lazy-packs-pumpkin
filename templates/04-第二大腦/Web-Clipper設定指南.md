---
title: Obsidian Web Clipper 設定指南
date: 2026-06-17
tags: [setup, web-clipper, clippings]
type: setup-guide
---

# Obsidian Web Clipper 設定指南

> 把瀏覽器上看到的好文章、影片、推文一鍵存進 vault 的 `Clippings/` 資料夾。

---

## 1. 安裝瀏覽器外掛

| 瀏覽器 | 下載連結 |
|--------|---------|
| Chrome | https://chromewebstore.google.com/detail/obsidian-web-clipper/cnjifjpddelmedmihgijeibhnjfabmlf |
| Firefox | https://addons.mozilla.org/firefox/addon/web-clipper-obsidian/ |
| Edge | 同 Chrome 商店連結（Edge 支援 Chrome 擴充功能） |
| Safari | https://apps.apple.com/app/obsidian-web-clipper/id6720708363 |

> 💡 Mac 上 Safari 跟 Chrome 都裝最方便：行動時用 Chrome，看 Apple News 用 Safari。

---

## 2. 設定外掛

1. 安裝完點擊瀏覽器右上角的 Web Clipper 圖示
2. 進入 **Settings**
3. **General** 分頁：
   - Language：**繁體中文（Traditional Chinese）**
4. **Vaults** 分頁 → 點 **+ Add vault**：
   - Name：`Pumpapa`
   - （保持其他預設）

---

## 3. 設定預設儲存資料夾（重點！）

進入 Settings → **Templates** 分頁，編輯 `Default` 模板：

| 欄位 | 填入 |
|------|------|
| **Vault** | Pumpapa |
| **Note location** | `Clippings/Articles/` |
| **Note name** | `{{title}}` |
| **Properties (frontmatter)** | 見下方範本 |
| **Note content** | 見下方範本 |

### Properties（frontmatter）範本

```yaml
title: {{title}}
date: {{date}}
tags:
  - clipping
source: {{url}}
author: {{author}}
type: clipping
status: 未消化
```

### Note content 範本

```markdown
# {{title}}

> **來源**：[{{url}}]({{url}})
> **作者**：{{author}}
> **抓取日期**：{{date}}
> **狀態**：未消化

## 摘要

{{description}}

## 原文內容

{{content}}

---

## 我的標記與感想

-

## 待延伸

-
```

---

## 4. 為 YouTube 影片建專屬模板（可選）

新建一個 Template 叫 `YouTube`：

| 欄位 | 填入 |
|------|------|
| **Template triggers** | URL contains `youtube.com/watch` |
| **Vault** | Pumpapa |
| **Note location** | `Clippings/Videos/` |
| **Note name** | `{{title}}` |

> 觸發條件設好之後，遇到 YouTube 影片自動切到 Videos 資料夾。

---

## 5. 使用流程

### 抓文章

1. 看到好文章 → 選取要的段落（也可全文）
2. 右鍵 → **Obsidian Web Clipper** → **Save List Page**
3. 跳出視窗確認 → 按 **Save**
4. 檔案自動建立在 `Clippings/Articles/`，iCloud 同步後 vault 看得到

### 抓 YouTube 字幕

1. 打開影片，往下捲到 **字幕記錄／顯示轉錄稿**
2. 點開字幕區
3. 在字幕區右鍵 → **Save List Page**
4. 檔案會包含影片網址、標題、完整字幕

### 抓沒字幕的影片（如演講）

1. 下載音檔（或用 OBS 錄音）
2. 在 Claude Code 對話框：

> 「請將這個音檔丟給 NotebookLM 抓逐字稿，逐字稿存到 `Clippings/Videos/[標題].md`」

3. Claude Code 會透過 NotebookLM MCP 處理（前提是有裝 NotebookLM MCP）

---

## 6. 抓進來後會怎樣？

- 檔案先在 `Clippings/Articles/`（或 Videos/Social），**frontmatter 的 `status: 未消化`**
- **AI 不會立刻處理**
- **每週日 9:17** 知識重整時，AI 會把所有 `未消化` 的 Clippings 提煉進 `知識庫/` 對應主題
- 提煉完原始檔留著不動，但 `status` 改成 `已消化`，並加上 `linked_to: [[知識庫/.../...]]`

---

## 7. 常見問題

| 問題 | 解法 |
|------|------|
| 抓完 vault 看不到 | iCloud 同步未完成，等幾秒；確認外掛 Vault 名稱拼對 |
| 中文網頁亂碼 | Web Clipper Settings → General → Language 改繁中 |
| 想改預設位置 | Templates → Default → Note location 改 |
| YouTube 沒抓到字幕 | 確認影片有字幕；先打開字幕區再 Save List Page |
| 抓進去檔名很亂 | 是因為原文標題就那樣；可手動改檔名，Obsidian wiki-link 會自動跟著更新 |

---

*南瓜實測整理 ・ 2026-06-17*

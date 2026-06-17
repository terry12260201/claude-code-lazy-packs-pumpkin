# 南瓜的 Claude Code 懶人包

> 個人實測整理的 Claude Code 設定懶人包，以 [@mathruffian-dot/claude-code-lazy-packs](https://github.com/mathruffian-dot/claude-code-lazy-packs) 為藍本，依照自己的使用情境（iCloud 同步、Windows + Mac 雙裝置、繁中台灣）修正與重寫。

---

## 使用方式

### 方式一：丟給 Claude Code 自動執行

把對應的 MD 檔複製到 Claude Code 桌面版或 CLI 對話框，貼上後 Claude 會逐步引導你完成。需要手動操作時會暫停告訴你。

### 方式二：當教學文件看

每份 MD 都當作純文件閱讀也可以，步驟清楚、有指令範例、有踩坑紀錄。

---

## 最低先備條件

- [ ] Claude 帳號（Pro 方案以上）
- [ ] Claude Code 桌面版或 CLI 已安裝
- [ ] 電腦能上網

---

## 懶人包清單

| 編號 | 名稱 | 版本 | 狀態 | 適用情境 |
|------|------|------|------|---------|
| 03 | [建立第二大腦 Obsidian × iCloud 版](./03-建立第二大腦-Obsidian-iCloud版.md) | v1.0 | ✅ Windows 實測通過 | iCloud 同步、Windows + Mac 雙裝置 |
| 04 | [第二大腦設定指南（三層結構 + 自動知識重整）](./04-第二大腦設定指南-三層結構.md) | v1.0 | ✅ Windows 實測通過 | 接續 #03，把 vault 升級成會自動成長的 AI 第二大腦 |

> 配套資源：
> - [`templates/CLAUDE-範本.md`](./templates/CLAUDE-範本.md) — 基礎 vault 班規
> - [`templates/CLAUDE-範本-三層版.md`](./templates/CLAUDE-範本-三層版.md) — 三層結構版（單一身份，例如老師）
> - [`templates/CLAUDE-範本-身份分流版.md`](./templates/CLAUDE-範本-身份分流版.md) — **多身份分流版**（推薦給創業者／斜槓）
> - [`templates/04-第二大腦/`](./templates/04-第二大腦/) — 六份模板（Clipping/創作/知識頁/週報/Web Clipper 指南/重整 Prompt）

> 待補：更多懶人包陸續整理中。

---

## 與原版的差異

本 repo 並非原版的 fork，而是針對個人使用情境的**重寫版**。主要差異：

| 面向 | 原版（@mathruffian-dot） | 本版 |
|------|----------------------|------|
| 主要訴求 | 教學工作流（老師為主） | 創作者／PM／XR 開發脈絡 |
| 同步方案 | Google Drive | iCloud（Apple 生態系） |
| 平台覆蓋 | Windows 為主 | Windows + Mac 平行對照 |
| MCP 設定 | 手動寫 settings.json（v0.5） | `claude mcp add -s user` 官方指令 |

> 推薦先看原版，建立對「懶人包」這個格式的理解，再依據自己的情境選用本版或自己改。

---

## 致謝

感謝 [@mathruffian-dot](https://github.com/mathruffian-dot) 製作並開源原始懶人包系列。本 repo 的格式、結構、敘事風格大量參考原版設計。

---

## 授權

MIT

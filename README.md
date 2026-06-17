# 南瓜的 Claude Code 懶人包

> 南瓜（陳南宏）個人實測迭代的 Claude Code 設定懶人包。針對 **iCloud 同步、Windows + Mac 雙裝置、多身份使用者**情境設計。

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
| 04 | [第二大腦設定指南（身份分流結構 + 自動知識重整）](./04-第二大腦設定指南-三層結構.md) | v1.0 | ✅ Windows 實測通過 | 接續 #03，把 vault 升級成會自動成長的 AI 第二大腦 |

> 配套資源：
> - [`templates/CLAUDE-範本.md`](./templates/CLAUDE-範本.md) — 基礎 vault 班規
> - [`templates/CLAUDE-範本-三層版.md`](./templates/CLAUDE-範本-三層版.md) — 三層結構版（單一身份）
> - [`templates/CLAUDE-範本-身份分流版.md`](./templates/CLAUDE-範本-身份分流版.md) — **多身份分流版**（推薦給創業者／斜槓）
> - [`templates/04-第二大腦/`](./templates/04-第二大腦/) — 六份模板（Clipping/創作/知識頁/週報/Web Clipper 指南/重整 Prompt）

> 待補：更多懶人包陸續整理中。

---

## 設計理念

本 repo 針對以下情境做了大量實測迭代：

| 面向 | 設計選擇 |
|------|---------|
| 同步方案 | **iCloud Drive**（適合已在 Apple 生態系的使用者） |
| 平台覆蓋 | **Windows + Mac 平行對照**，每個指令兩平台都驗證 |
| MCP 設定 | 使用 `claude mcp add -s user` 官方指令（user scope） |
| 資料結構 | **身份分流**架構，適合多重身份的創業者或斜槓 |
| 自動化 | 每週日定時知識重整，產出週報 |

---

## 授權

MIT

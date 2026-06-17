---
title: 'Claude Code 懶人包 #03：建立第二大腦（Obsidian × iCloud 版）'
date: '2026-06-17'
type: 懶人包
version: v1.0
status: 實測通過（Windows）
tags:
  - Claude-Code
  - 懶人包
  - Obsidian
  - MCP
  - 第二大腦
  - iCloud
author: 南瓜（陳南宏）
---

# Claude Code 懶人包 #03：建立第二大腦（Obsidian × iCloud 版）

> 版本：v1.0
> 更新日期：2026-06-17
> 適用情境：使用 **iCloud Drive** 同步 Obsidian 筆記庫的 **Windows + Mac** 雙裝置使用者

> 📌 **本懶人包可獨立執行**：會自動檢查環境並安裝所需工具，不需要先看過其他懶人包。

---

## 設計重點

本懶人包針對以下三個情境做了實測迭代：

| 面向 | 設計選擇 |
|------|---------|
| 同步方案 | **iCloud Drive**（Mac + iPhone + Windows 三平台一份檔案） |
| MCP 註冊方式 | `claude mcp add -s user` 官方指令（user scope，全專案通用） |
| 平台覆蓋 | **Windows + Mac 平行對照**，每個指令兩平台都寫好 |

---

## 這個懶人包會幫你做什麼？

幫你的 Claude Code 桌面版（或 CLI）裝上「第二大腦」，完成後 Claude 可以：

- 讀取你 Obsidian 筆記庫裡的所有筆記
- 幫你搜尋、新增、編輯、移動筆記
- 管理 frontmatter 與 tag
- 在 **Windows + Mac + iPhone** 三裝置共用同一份筆記庫

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版或 CLI 已安裝（Pro 方案以上）
- [ ] 已有 Apple ID 並登入 iCloud
- [ ] 電腦有網路連線
- [ ] Node.js 已安裝（沒裝也沒關係，步驟零會自動安裝）

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。

---

## 階段一：環境檢查

### 步驟零：環境檢查（必做，不要跳過）

> 請 Claude 在開始前，先自動確認以下所有項目。
> **不要假設環境正常**。任何一項不符合請先解決再繼續。

#### 1. 確認作業系統

執行系統指令確認是 Windows / macOS。本懶人包**不適用 Linux**（iCloud 在 Linux 上沒有官方支援）。

#### 2. 確認網路連線

#### 3. 檢查 Node.js

```bash
# Windows / Mac 通用
node --version
npm --version
```

若未安裝：

| 平台 | 安裝指令 |
|------|---------|
| Windows | `winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements` |
| Mac | `brew install node`（需先有 Homebrew） |

#### 4. 檢查 Claude Code CLI

```bash
# Windows
where.exe claude

# Mac
which claude
```

> 💡 本懶人包需要 `claude` CLI 指令。如果你只裝了 Claude Code 桌面版而沒有 CLI，請先安裝 CLI 版本（`npm install -g @anthropic-ai/claude-code`）。

#### 5. 檢查 iCloud Drive 是否已同步

| 平台 | 確認方式 |
|------|---------|
| Windows | 確認 `C:\Users\[使用者]\iCloudDrive\` 路徑存在 |
| Mac | Finder 側欄能看到 iCloud Drive，且 `~/Library/Mobile Documents/` 內有資料夾 |

> ⚠️ **iCloud for Windows 必須先安裝並登入**。若沒有，請到 https://support.apple.com/zh-tw/118279 下載。

#### 6. 檢查 Obsidian 是否已安裝

| 平台 | 確認方式 |
|------|---------|
| Windows | `where.exe obsidian` 或檔案總管找 `C:\Users\[使用者]\AppData\Local\Programs\Obsidian\Obsidian.exe` |
| Mac | `ls /Applications/Obsidian.app` |

> 全部通過後，告知使用者環境狀態。
> 如果有不通過的項目，列出問題清單並逐一引導解決。

---

### 步驟一：安裝 Obsidian（如果未安裝）

> 🖐️ **需要手動操作**：
> 1. 開啟瀏覽器，到 https://obsidian.md 下載對應平台安裝檔
> 2. 執行安裝檔，按指示完成
> 3. 安裝完先不要開啟，等下一步建好 vault 再開

---

### 步驟二：安裝 iCloud（如果未安裝）

> 🖐️ **需要手動操作**：
>
> **Mac**：iCloud 內建，到「系統設定 → Apple ID → iCloud Drive」開啟即可。
>
> **Windows**：
> 1. 到 https://support.apple.com/zh-tw/118279 下載 iCloud for Windows
> 2. 用你的 Apple ID 登入
> 3. 勾選「iCloud Drive」開啟同步
> 4. 等待初次同步完成（檔案總管側欄會出現 iCloud Drive 資料夾）

---

## 階段二：建立 Vault

### 步驟三：建立 Obsidian Vault 資料夾

> 🖐️ **需要手動操作**：請決定 vault 名稱（範例使用 `Pumpapa`，你可以換成任何名稱）。

#### Obsidian + iCloud 的特殊路徑

Obsidian 行動版（iOS）使用 Apple 規定的 iCloud 容器名稱 `iCloud~md~obsidian`，這個名稱**不能改**。要讓三裝置都看到同一個 vault，**必須建在這個容器內**。

| 平台 | Vault 完整路徑 |
|------|--------------|
| **Windows** | `C:\Users\[使用者]\iCloudDrive\iCloud~md~obsidian\[vault名稱]\` |
| **Mac** | `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]/` |
| **iOS** | Files App → iCloud Drive → Obsidian → [vault名稱] |

> 💡 Mac 路徑多一層 `Documents/`，這是 iCloud 在 Mac 上的標準結構，**不要少這層**。

#### 建議的資料夾結構

```
[vault名稱]/
├── CLAUDE.md           ← 班規（步驟七建立）
├── _SETUP-xxx.md       ← 環境建置記錄
├── Attachments/        ← 圖片附件
├── Daily/              ← 每日筆記
├── Projects/           ← 專案
├── Resources/          ← 資料、素材
└── Templates/          ← 模板
```

> 一個資料夾叫什麼、要不要分主題，自己決定。Claude 會依照你 CLAUDE.md 寫的規則決定新筆記放哪。

---

### 步驟四：用 Obsidian 開啟 Vault

> 🖐️ **需要手動操作**：
> 1. 開啟 Obsidian
> 2. 選擇「Open folder as vault」
> 3. 指向剛剛建立的 vault 資料夾
> 4. Obsidian 會建立 `.obsidian/` 設定資料夾並打開空筆記庫

---

## 階段三：連接 MCP（讓 Claude Code 能讀寫筆記）

### 步驟五：全域安裝 mcpvault

mcpvault 是讓 Claude Code 能搜尋、讀寫筆記的 MCP server。
**不需要 Obsidian 開著就能運作。不需要在 Obsidian 內裝任何外掛。**

```bash
# Windows / Mac 通用
npm install -g @bitbonsai/mcpvault
```

確認安裝位置：

| 平台 | 確認指令 | 預期路徑 |
|------|---------|---------|
| Windows | `where.exe mcpvault` | `C:\Users\[使用者]\AppData\Roaming\npm\mcpvault.cmd` |
| Mac | `which mcpvault` | `/usr/local/bin/mcpvault` 或 `/opt/homebrew/bin/mcpvault` |

---

### 步驟六：用 `claude mcp add` 註冊 MCP

> ⚠️ **重要修正（vs. 原版 v0.5）**
>
> 原版教你把 `mcpServers` 寫進 `~/.claude/settings.json`。
> **這個欄位在目前版本的 Claude Code 中已不被接受**，會被 schema validator 拒絕。
>
> ✅ 正確做法：使用 `claude mcp add -s user` 指令，會自動寫入 `~/.claude.json`（注意是 dotfile，不是 settings.json）。

#### Windows 指令

```bash
claude mcp add -s user obsidian "C:\Users\[使用者]\AppData\Roaming\npm\mcpvault.cmd" "C:\Users\[使用者]\iCloudDrive\iCloud~md~obsidian\[vault名稱]"
```

#### Mac 指令

```bash
claude mcp add -s user obsidian mcpvault "$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]"
```

> 💡 `-s user` 代表 user scope：在**任何專案**開啟 Claude Code 都會載入這個 MCP，不需要每個專案重設。

---

### 步驟七：驗證 MCP 連線

```bash
claude mcp list
```

成功的話會看到：

```
obsidian: <mcpvault path> <vault path> - ✓ Connected
```

進一步測試（直接呼叫 mcpvault 看工具清單）：

```bash
# Windows
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | "C:\Users\[使用者]\AppData\Roaming\npm\mcpvault.cmd" "C:\Users\[使用者]\iCloudDrive\iCloud~md~obsidian\[vault名稱]"

# Mac
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | mcpvault "$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]"
```

如果能回傳工具清單 JSON，代表 mcpvault 與 vault 都正常。

---

### 步驟八：重啟 Claude Code

> 🖐️ **需要手動操作**：完全關閉 Claude Code 桌面版，再重新開啟。

重啟後新對話中應該出現 `mcp__obsidian__*` 系列工具。隨便試一句：

> 「列出我 vault 根目錄」

Claude 會用 `mcp__obsidian__list_directory` 回傳資料夾與檔案。

---

## 階段四：建立 vault 的「班規」

### 步驟九：建立 CLAUDE.md

在 vault 根目錄建立 `CLAUDE.md`，每次 Claude 進入 vault 都會自動讀。

> 🖐️ **需要手動操作**：請 Claude 幫你詢問以下資訊，再寫入 CLAUDE.md：
> 1. 你的身份、職業、主要工作領域
> 2. 預設語言（建議：繁體中文）
> 3. Vault 的資料夾用途
> 4. 新增筆記時的格式偏好（frontmatter、命名、tag 規則）
> 5. 任何長期會出現的脈絡（公司、家庭、長期專案等）

範例可參考本 repo 內的 [CLAUDE-範本.md](./templates/CLAUDE-範本.md)。

---

### 步驟十：建立第一篇正式筆記

請 Claude 幫你建立一篇有意義的起手筆記，例如：
- 「本月專案列表」
- 「我的工具清單」
- 「靈感蒐集」

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Obsidian 會做的事 |
|----------|--------------------------|
| 「搜尋我的筆記有沒有跟 XXX 相關的」 | 用 `search_notes` 全文搜尋，回傳相關筆記 |
| 「幫我新增一篇筆記紀錄今天的會議」 | 用 `write_note` 建立筆記，含 frontmatter |
| 「幫我看 XXX 這篇筆記，延伸出 5 個點子」 | 用 `read_note` 讀取後生成延伸建議 |
| 「把這篇筆記裡的『A』全部替換成『B』」 | 用 `patch_note` 局部替換 |
| 「列出 vault 內所有 tag」 | 用 `list_all_tags` 統計 |
| 「把今天對話的重點存進 Daily/」 | 用 `write_note` 寫到指定資料夾 |

---

## mcpvault 提供的 15 個工具

| 工具 | 用途 |
|------|------|
| `read_note` / `read_multiple_notes` | 讀單篇／批次（最多 10 篇） |
| `write_note` | 新增或覆寫（支援 append / prepend 模式） |
| `patch_note` | 局部替換特定字串 |
| `search_notes` | 全文／frontmatter 搜尋 |
| `list_directory` | 列出資料夾內容 |
| `move_note` / `move_file` | 移動或重新命名 |
| `delete_note` | 刪除（需確認路徑） |
| `update_frontmatter` / `get_frontmatter` | frontmatter 操作 |
| `manage_tags` / `list_all_tags` | 標籤管理 |
| `get_notes_info` | 取得 metadata 不讀內容 |
| `get_vault_stats` | Vault 統計（總數、大小、最近修改） |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：

> 「Obsidian 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

或手動重置：

```bash
# 1. 移除 MCP 註冊
claude mcp remove obsidian

# 2.（可選）重裝 mcpvault
npm uninstall -g @bitbonsai/mcpvault
npm install -g @bitbonsai/mcpvault

# 3. 從步驟六重新註冊
claude mcp add -s user obsidian <mcpvault 路徑> <vault 路徑>

# 4. 驗證
claude mcp list
```

---

## 常見問題

| 問題 | 解法 |
|------|------|
| `claude mcp list` 顯示 `✗ Failed to connect` | 確認 vault 路徑存在且 mcpvault 可執行；移除後重新註冊 |
| 重啟後找不到 `mcp__obsidian__*` 工具 | 確認 user scope 註冊成功；Mac 可能需要重開終端讓 PATH 生效 |
| Mac `which mcpvault` 找不到 | `npm config get prefix` 看 npm 全域 bin 位置，加入 `$PATH` |
| iCloud 路徑找不到 vault | Mac 上 vault 在 `Documents/` 子資料夾內，**不要少這層** |
| Obsidian 開不到 vault | 用「Open folder as vault」指向 `[vault名稱]/` 整個資料夾，不是 `.obsidian/` |
| Windows iCloud 同步很慢 | 在系統匣 iCloud 圖示確認同步進度；大量檔案首次同步可能要等 |
| 多裝置編輯衝突，出現 `xxx 2.md` | 手動合併內容後刪除冗餘檔；避免同時在兩台裝置編輯同一篇 |
| `.obsidian/workspace.json` 一直衝突 | Obsidian → 設定 → 一般 → 關掉「啟動時還原前一個工作區」 |
| `mcpServers` 寫到 settings.json 被拒絕 | 那是原版 v0.5 的舊做法，**改用** `claude mcp add` |

---

## 跨平台同步注意事項

### ✅ 會透過 iCloud 自動同步的
- 所有 `.md` 筆記內容
- `CLAUDE.md`、`_SETUP` 等設定文件
- `Attachments/` 內的圖片附件

### ❌ 不會自動同步、要每台機器分別設定的
- mcpvault 全域安裝（每台 `npm install -g` 一次）
- Claude Code 的 MCP 註冊（每台 `claude mcp add` 一次）
- Obsidian 桌面版本身
- Node.js / Claude Code CLI

### ⚠️ 容易踩雷的同步點
- `.obsidian/workspace.json` 多裝置衝突 → 關閉「還原工作區」
- iCloud 同步延遲 → 切裝置前確認系統匣同步圖示已完成
- 大量附件首次同步 → 必要時手動觸發下載

---

## 同步方案對照

本懶人包預設 **iCloud**，如果你要改用其他方案：

| 方案 | Vault 路徑大致位置 | 適合 |
|------|------------------|------|
| **iCloud**（本懶人包） | `iCloud~md~obsidian/` 容器內 | Mac + iPhone + Windows 使用者 |
| **Google Drive** | `G:\我的雲端硬碟\` 或 `~/Library/CloudStorage/GoogleDrive-*/` | 跨 Google 生態系 |
| **Obsidian Sync** | 任意位置，Obsidian 內設定（$4/月） | 不想依賴第三方雲端 |
| **OneDrive / Dropbox** | 對應同步資料夾 | 已有訂閱者 |

> 差別只在 vault 路徑不同，MCP 連接指令的後半段換成對應路徑即可。

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-06-17 | v1.0 | 初版。基於原作者 v0.5 修正：(1) 改用 iCloud (2) MCP 改用 `claude mcp add` (3) Windows + Mac 平行對照。Windows 端已實測通過。 |

---

## 相關連結

- [mcpvault GitHub](https://github.com/bitbonsai/mcpvault)
- [Obsidian 官網](https://obsidian.md)
- [iCloud for Windows 下載](https://support.apple.com/zh-tw/118279)
- [Claude Code 文件](https://docs.claude.com/claude-code)

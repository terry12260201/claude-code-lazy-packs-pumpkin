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
---

# Claude Code 懶人包 #03：建立第二大腦（Obsidian × iCloud 版）

> 版本：v1.0
> 更新日期：2026-06-17
> 適用情境：使用 **iCloud Drive** 同步 Obsidian 筆記庫的 **Windows + Mac** 雙裝置使用者

> 📌 **本懶人包可獨立執行**：會自動檢查並安裝所需工具，不需要先看過其他懶人包。你只要確認下方「先備條件」即可開始。

---

## 這個懶人包會幫你做什麼？

幫你的 Claude Code 裝上「第二大腦」，完成後 Claude 可以：

- 讀取你 Obsidian 筆記庫裡的所有筆記
- 幫你搜尋、新增、編輯、移動、刪除筆記
- 管理 frontmatter（標題、日期、tag）
- 在 **Windows + Mac + iPhone** 三裝置之間透過 iCloud 共用同一份筆記庫
- 不需要在 Obsidian 內裝任何外掛、也不需要 Obsidian 開著就能運作

---

## 先備條件

在使用這個懶人包之前，請確認：

- [ ] Claude Code 桌面版或 CLI 已安裝且能正常使用（Pro 方案以上）
- [ ] 已有 Apple ID 並登入 iCloud（這份懶人包預設用 iCloud 同步）
- [ ] 電腦有網路連線
- [ ] Node.js 已安裝（mcpvault 需要 npx 指令）— 沒裝也沒關係，步驟零會自動檢查並安裝

---

## 請 Claude Code 幫我執行以下步驟

> ⚠️ 以下內容是給 Claude Code 讀的操作指令。
> 你只需要把這整份 MD 檔丟給 Claude Code 桌面版的 Code 分頁，它會自動開始執行。
> 遇到需要你手動操作的地方，它會暫停並告訴你該怎麼做。
>
> **所有安裝與設定都在 Claude Code 桌面版內完成，不需要另外打開 PowerShell 或命令提示字元。**
> 如果 Claude Code 桌面版無法執行某個指令，才會引導你到終端機操作。
> 進階使用者也可以直接使用 Claude Code CLI 版本執行本懶人包。

---

## 階段一：環境檢查與基礎工具

### 步驟零：環境檢查（必做，不要跳過）

> 請 Claude 在開始前，先自動確認以下所有項目。
> 如果有任何一項不符合，請先告知使用者問題所在，並引導解決後再繼續。
> **不要假設環境正常，每一項都要實際執行指令確認**。

#### 0-1. 確認作業系統

執行系統指令確認是 **Windows / macOS**。本懶人包**不適用 Linux**（iCloud 在 Linux 上沒有官方支援，要用 Linux 請改用 Google Drive 或 Syncthing 方案）。

後續所有指令請根據實際的作業系統選擇正確版本執行。

#### 0-2. 確認網路連線

執行任意網路指令確認（例如 `ping -n 2 github.com` / `ping -c 2 github.com`）。沒網路後面什麼都不用做。

#### 0-3. 檢查 Node.js 是否已安裝

⚠️ **Windows 重要提醒**：Claude Code 桌面版的 bash 環境**可能找不到 `node`，即使已安裝**。這是因為 Windows 的 PATH 沒被 bash shell 繼承。請用以下順序檢查：

```bash
# 第一次嘗試
node --version

# 若失敗，補 PATH 再試
export PATH="/c/Program Files/nodejs:$PATH" && node --version

# 若仍失敗，代表真的未安裝
```

若未安裝，依平台安裝：

| 平台 | 安裝指令 |
|------|---------|
| Windows | `winget install --id OpenJS.NodeJS --accept-source-agreements --accept-package-agreements` |
| Mac | `brew install node`（需先有 Homebrew） |

> 💡 安裝完成後，Windows 環境下後續所有 `node` / `npm` / `npx` 指令都需要先加上 `export PATH="/c/Program Files/nodejs:$PATH"`（僅 Windows）。Mac 不需要。

#### 0-4. 檢查 npx 可用

```bash
npx --version
```

Windows 記得加 PATH。回傳版本號代表 OK。

#### 0-5. 檢查 Claude Code CLI 是否可用

```bash
# Windows
where.exe claude

# Mac
which claude
```

> 💡 本懶人包**需要 `claude` CLI 指令**（用來執行 `claude mcp add`）。
>
> - 如果你只裝了 Claude Code 桌面版而沒有 CLI，請先安裝 CLI 版本：
>   ```bash
>   npm install -g @anthropic-ai/claude-code
>   ```
> - 如果連桌面版都沒有，先到 https://claude.ai/code 下載

#### 0-6. 檢查 iCloud Drive 是否已同步

| 平台 | 確認方式 |
|------|---------|
| **Windows** | 確認 `C:\Users\[你的使用者名稱]\iCloudDrive\` 路徑存在 |
| **Mac** | Finder 側欄能看到「iCloud Drive」，且 `~/Library/Mobile Documents/` 內有資料夾 |

⚠️ **Windows 必須先安裝 iCloud for Windows 並登入**。若 iCloudDrive 資料夾不存在，請先到 https://support.apple.com/zh-tw/118279 下載 iCloud for Windows，用 Apple ID 登入後勾選 iCloud Drive 同步，等待初次同步完成。

#### 0-7. 檢查 Obsidian 是否已安裝

| 平台 | 確認方式 |
|------|---------|
| **Windows** | `where.exe obsidian`，或檔案總管確認 `C:\Users\[你]\AppData\Local\Programs\Obsidian\Obsidian.exe` 存在 |
| **Mac** | `ls /Applications/Obsidian.app`，或 Launchpad 看得到 Obsidian 圖示 |

#### 環境檢查總結

> 全部通過後，告知使用者環境狀態：
>
> ```
> ✅ 環境檢查通過：
>    - OS: Windows 10 / macOS
>    - Node.js: vXX.X.X
>    - Claude CLI: 已安裝
>    - iCloud Drive: 已同步
>    - Obsidian: 已安裝
> 準備進入階段二。
> ```
>
> 如果有不通過的項目，列出問題清單並逐一引導解決後才繼續。

---

### 步驟一：安裝 Obsidian（如果未安裝）

> 如果步驟零確認 Obsidian 已安裝，跳過此步驟。

> 🖐️ **需要手動操作**：
>
> 1. 開啟瀏覽器，前往 https://obsidian.md
> 2. 點擊「Download」下載對應平台安裝檔
> 3. 執行安裝檔，按指示完成（一路下一步即可）
> 4. 安裝完成後**先不要開啟 Obsidian**，等下一步建好 vault 資料夾再開

---

### 步驟二：安裝並設定 iCloud Drive（如果未設定）

> 如果步驟零確認 iCloud 已同步，跳過此步驟。

iCloud Drive 會在你的電腦上建立一個同步資料夾，Obsidian 的筆記存在這裡就能自動同步到 Mac、iPhone、iPad，換裝置也不會遺失。

#### Mac 端

> 🖐️ **需要手動操作**：
>
> 1. 開啟「系統設定」→「Apple ID」→「iCloud」
> 2. 開啟「iCloud Drive」
> 3. 點擊「同步此 Mac」→「打開」
> 4. 確認 Finder 側欄能看到「iCloud Drive」

#### Windows 端

> 🖐️ **需要手動操作**：
>
> 1. 開啟瀏覽器，前往 https://support.apple.com/zh-tw/118279
> 2. 點擊「下載 iCloud for Windows」（會導向 Microsoft Store 或下載 .exe）
> 3. 安裝完成後開啟，用你的 Apple ID 登入
> 4. 在設定畫面勾選「iCloud Drive」
> 5. 等待初次同步完成（系統匣會出現 iCloud 圖示）
> 6. 確認檔案總管左側出現「iCloud Drive」項目
> 7. 進入該資料夾，確認路徑類似 `C:\Users\[你]\iCloudDrive\`

> 💡 確認能看到 iCloud Drive 資料夾後，告訴 Claude「完成了」即可繼續。

---

## 階段二：建立 Vault 資料夾

### 步驟三：建立 Obsidian Vault 資料夾

> 🖐️ **需要手動操作**：請決定 vault 名稱（例如 `secondbrain`、`MyVault`、你的英文暱稱等，可隨意取）。

#### Obsidian × iCloud 的特殊路徑（很重要）

Obsidian 行動版（iOS）使用 Apple 規定的 iCloud 容器名稱 `iCloud~md~obsidian`，這個資料夾名稱**是固定的不能改**。要讓 Mac、Windows、iPhone 三裝置看到同一個 vault，**vault 必須建在這個容器資料夾內**。

| 平台 | Vault 完整路徑 |
|------|--------------|
| **Windows** | `C:\Users\[你]\iCloudDrive\iCloud~md~obsidian\[vault名稱]\` |
| **Mac** | `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]/` |
| **iOS** | Files App → iCloud Drive → Obsidian → [vault名稱] |

> ⚠️ **Mac 路徑多一層 `Documents/`**，這是 iCloud 在 Mac 上的標準結構，**不要少這層**。少了就跟 Windows、iOS 對不上。

#### 第一次建立的情境

如果你**之前沒在 iOS 用過 Obsidian**：

- Windows：直接在 `C:\Users\[你]\iCloudDrive\` 底下手動新建 `iCloud~md~obsidian` 資料夾，再進去新建 `[vault名稱]`
- Mac：第一次開 Obsidian iOS 版時，App 會自動建立 `iCloud~md~obsidian` 容器；或在 Mac 上手動建立 `~/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]/`

如果你**已經在 iOS 用過 Obsidian**：

- 那個 `iCloud~md~obsidian` 資料夾應該已存在，直接在裡面建你的 vault 子資料夾就好。

#### 建議的初始資料夾結構

```
[vault名稱]/
├── CLAUDE.md           ← 班規檔（階段四會建立）
├── Attachments/        ← 圖片附件
├── Daily/              ← 每日筆記（可選）
├── Projects/           ← 專案（可選）
├── Resources/          ← 資料、素材（可選）
└── Templates/          ← 模板（可選）
```

> 💡 子資料夾要分什麼名字、要不要分主題，自己決定。Claude 會依照 CLAUDE.md 寫的規則決定新筆記放哪。
>
> 進階使用者請參考 [懶人包 #04](./04-第二大腦設定指南-三層結構.md)，那裡有「身份分流」與「三層結構」兩種更完整的設計可選。

建立完成後，**記錄 vault 的完整路徑**（後續步驟會用到）。

---

### 步驟四：用 Obsidian 開啟 Vault

> 🖐️ **需要手動操作**：
>
> 1. 開啟 Obsidian
> 2. 在歡迎畫面選擇「**Open folder as vault**」（開啟資料夾作為筆記庫）
> 3. 指向剛才建立的 vault 資料夾
> 4. 信任作者（Trust author）→ Open
> 5. Obsidian 會建立 `.obsidian/` 設定資料夾並打開空筆記庫

#### 推薦的 Obsidian 基本設定（避免後續踩雷）

進入 Obsidian → 設定（齒輪圖示）：

| 路徑 | 設定 | 為什麼 |
|------|------|--------|
| 一般 → 啟動時開啟前一個工作區 | **關閉** | 避免 `.obsidian/workspace.json` 在多裝置間衝突 |
| 編輯器 → 預設新筆記位置 | **與目前檔案相同的資料夾** | 在哪個資料夾建新筆記就放哪 |
| 檔案與連結 → 新附件位置 | **指定資料夾**：`Attachments` | 圖片統一放這 |
| 外觀 → 介面字體 | 中文字型優先（如 Noto Sans TC） | 避免中文字體跑版 |

確認使用者已成功開啟 vault 後，繼續下一階段。

---

## 階段三：連接 MCP（讓 Claude Code 能讀寫筆記）

### 步驟五：全域安裝 mcpvault

**mcpvault** 是讓 Claude Code 能搜尋、讀取、編輯你 Obsidian 筆記的 MCP server。

- **不需要 Obsidian 開著**就能運作
- **不需要在 Obsidian 內安裝任何外掛**
- 直接讀寫 vault 資料夾內的 `.md` 檔案

#### 安裝指令

```bash
# Windows（記得加 PATH）
export PATH="/c/Program Files/nodejs:$PATH" && npm install -g @bitbonsai/mcpvault

# Mac
npm install -g @bitbonsai/mcpvault
```

安裝會花 5–10 秒，看到 `added XXX packages` 代表完成。

#### 確認安裝位置

安裝完成後，確認 mcpvault 實際路徑（後面註冊 MCP 時要填）：

| 平台 | 確認指令 | 預期路徑 |
|------|---------|---------|
| **Windows** | `where.exe mcpvault` | `C:\Users\[你]\AppData\Roaming\npm\mcpvault.cmd` |
| **Mac**（Intel） | `which mcpvault` | `/usr/local/bin/mcpvault` |
| **Mac**（Apple Silicon） | `which mcpvault` | `/opt/homebrew/bin/mcpvault` |
| **Mac**（自訂 prefix） | `which mcpvault` | `~/.npm-global/bin/mcpvault`（依設定而定） |

⚠️ **Windows 注意**：路徑結尾是 `.cmd`，不是 `.exe`。寫設定時要寫全 `mcpvault.cmd`。

---

### 步驟六：用 `claude mcp add` 註冊 MCP

> ⚠️ **重要說明**：
>
> 註冊 MCP 不要手動編輯 `~/.claude/settings.json`！
> 目前版本的 Claude Code 的 `settings.json` schema **不接受 `mcpServers` 欄位**，寫了會被驗證器拒絕。
>
> ✅ 正確做法：用官方指令 `claude mcp add -s user`，會自動寫入 `~/.claude.json`（注意是 dotfile，不是 settings.json）。

#### 註冊指令

#### Windows

```bash
claude mcp add -s user obsidian "C:\Users\[你]\AppData\Roaming\npm\mcpvault.cmd" "C:\Users\[你]\iCloudDrive\iCloud~md~obsidian\[vault名稱]"
```

把 `[你]` 改成你的使用者名稱，`[vault名稱]` 改成你的 vault 名稱。

#### Mac

```bash
claude mcp add -s user obsidian mcpvault "$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]"
```

> 💡 Mac 不用寫完整路徑因為 `mcpvault` 已在 PATH 裡，直接用指令名即可。

#### 指令說明

| 參數 | 用途 |
|------|------|
| `-s user` | **user scope**：在任何專案開啟 Claude Code 都會載入這個 MCP，不需要每個專案重設 |
| `obsidian` | 這個 MCP 的名稱（之後工具會叫 `mcp__obsidian__read_note` 等） |
| 第一個位置參數 | mcpvault 可執行檔的完整路徑 |
| 第二個位置參數 | 你 vault 的完整路徑 |

#### 成功訊息

執行後應該看到：

```
Added stdio MCP server obsidian with command: ... to user config
File modified: C:\Users\[你]\.claude.json
```

---

### 步驟七：驗證 MCP 連線

#### 驗證方式 1：用 `claude mcp list`

```bash
claude mcp list
```

成功訊息：

```
Checking MCP server health...

obsidian: <mcpvault 路徑> <vault 路徑> - ✓ Connected
```

看到 ✓ Connected 代表 MCP server 啟動 OK 且能讀到你的 vault。

#### 驗證方式 2：直接 ping mcpvault（進階）

如果想確認 mcpvault 本身正確讀到 vault 內容：

```bash
# Windows
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | "C:\Users\[你]\AppData\Roaming\npm\mcpvault.cmd" "C:\Users\[你]\iCloudDrive\iCloud~md~obsidian\[vault名稱]"

# Mac
echo '{"jsonrpc":"2.0","id":1,"method":"tools/list"}' | mcpvault "$HOME/Library/Mobile Documents/iCloud~md~obsidian/Documents/[vault名稱]"
```

成功的話會回傳一大串 JSON，裡面列出 15 個 mcpvault 工具（read_note、write_note、search_notes、list_directory 等）。看到這個代表 mcpvault 本身 100% 正常，問題（如果有）就只可能在 Claude Code 那端。

#### 驗證方式 3：建立測試筆記

連接成功後，重啟 Claude Code 並在對話中輸入：

> 「在我的 vault 根目錄建立一篇測試筆記，檔名 `測試.md`，內容：『MCP 連線成功！』，含 frontmatter（title、date、tags）」

Claude 會用 `mcp__obsidian__write_note` 工具建立檔案。回到 Obsidian 看，應該看到新筆記。確認後可以刪除。

---

### 步驟八：重啟 Claude Code

> 🖐️ **需要手動操作**：完全關閉 Claude Code 桌面版（不是最小化），再重新開啟。
>
> 重啟原因：MCP server 的工具清單只在啟動時載入。新註冊的 MCP 必須重啟才能用。

重啟後新對話中應該出現 `mcp__obsidian__*` 系列工具。隨便試一句：

> 「列出我 vault 根目錄」

Claude 會用 `mcp__obsidian__list_directory` 回傳資料夾與檔案清單。看到回傳就代表全部設定成功。

---

## 階段四：建立 CLAUDE.md「班規」

### 步驟九：建立 vault 的 CLAUDE.md

在 vault 根目錄建立 `CLAUDE.md`，**每次 Claude 進入 vault 都會自動讀**。這份檔案決定 Claude 怎麼跟你 vault 互動。

> 🖐️ **需要手動操作**：請 Claude 幫你詢問以下資訊，再寫入 CLAUDE.md：
>
> 1. 你的姓名／暱稱、職業、主要工作領域
> 2. 預設語言（建議：繁體中文，台灣用語）
> 3. Vault 的資料夾用途
> 4. 新增筆記時的格式偏好（frontmatter 欄位、檔名規則、tag 規則）
> 5. 任何長期會出現的脈絡（公司、家庭、長期專案等）
> 6. 想要的回答風格（簡潔／詳細／條列／敘述）

CLAUDE.md 範本可以參考本 repo 的 [`templates/CLAUDE-範本.md`](./templates/CLAUDE-範本.md)。

#### 最簡 CLAUDE.md 範例

```markdown
# [Vault 名稱] — CLAUDE.md

## 關於我
- 姓名／暱稱：[填寫]
- 職業：[填寫]
- 工作領域：[填寫，1-3 項]

## 語言與風格
- 預設語言：繁體中文（台灣用語）
- 風格：簡潔、有條理、可直接執行

## Vault 結構
- Daily/ — 每日筆記
- Projects/ — 專案
- Resources/ — 素材
- Templates/ — 模板
- Attachments/ — 圖片

## 寫筆記規則
1. 一律加 frontmatter（title、date、tags）
2. 標題用 H1
3. 連結用 [[wiki-link]]
4. 圖片放 Attachments/
```

> 💡 進階規則（例如「身份分流」「三層結構」「每週知識重整」）請看 [懶人包 #04](./04-第二大腦設定指南-三層結構.md)。

---

### 步驟十：建立第一篇正式筆記

請 Claude 幫你建立一篇有意義的起手筆記，例如：

- 「本月專案總覽」
- 「我的工作流與工具清單」
- 「想做但還沒做的清單」

> 🖐️ **需要手動操作**：告訴 Claude 你想記錄什麼主題的第一篇筆記。

---

## 完成！接下來你可以這樣用

| 你說的話 | Claude + Obsidian 會做的事 |
|----------|--------------------------|
| 「搜尋我的筆記有沒有跟 XXX 相關的」 | 用 `search_notes` 全文搜尋，回傳相關筆記 |
| 「幫我新增一篇筆記，紀錄今天的會議」 | 用 `write_note` 建立筆記，含 frontmatter |
| 「幫我整理 X 這篇筆記的重點」 | 讀取後做摘要 |
| 「把這篇筆記裡的 A 全部替換成 B」 | 用 `patch_note` 局部替換 |
| 「列出我所有 #ai 標籤的筆記」 | 用 `manage_tags` + `list_directory` 找 |
| 「幫我把今天對話的重點存到 Daily/」 | 用 `write_note` 寫到指定資料夾 |
| 「我的 vault 有幾篇筆記？」 | 用 `get_vault_stats` 回傳統計 |

---

## mcpvault 提供的 15 個工具

完整工具列表（重啟 Claude Code 後可使用）：

| 工具 | 用途 |
|------|------|
| `read_note` | 讀單篇筆記 |
| `read_multiple_notes` | 批次讀（最多 10 篇） |
| `write_note` | 新增或覆寫（支援 overwrite / append / prepend） |
| `patch_note` | 局部替換特定字串（小編輯比 write 有效率） |
| `search_notes` | 全文／frontmatter 搜尋 |
| `list_directory` | 列出資料夾內容 |
| `move_note` | 移動或重新命名 .md 筆記 |
| `move_file` | 移動或重新命名任何檔案 |
| `delete_note` | 刪除筆記（需確認路徑） |
| `update_frontmatter` | 更新 frontmatter 不動內文 |
| `get_frontmatter` | 取得 frontmatter |
| `get_notes_info` | 取得 metadata 不讀內容 |
| `manage_tags` | 新增／移除／列出 tags |
| `list_all_tags` | 列出全 vault 所有 tag 與出現次數 |
| `get_vault_stats` | Vault 統計（總數、大小、最近修改） |

---

## 如果安裝失敗，如何重來

對 Claude Code 說：

> 「Obsidian 懶人包執行失敗了，幫我檢查哪裡出問題，重新處理。」

Claude 會自動：

1. 跑 `claude mcp list` 看 MCP 狀態
2. 確認 vault 路徑可存取
3. 重新註冊 MCP
4. 重新驗證

### 完全重置 MCP 連接

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
| `claude mcp list` 顯示 `✗ Failed to connect` | 1) 確認 vault 路徑存在 2) 確認 mcpvault 可執行檔存在 3) 用方式 2 直接 ping 看 mcpvault 是否回傳 JSON |
| 重啟後找不到 `mcp__obsidian__*` 工具 | 1) 確認用的是 user scope（`-s user`） 2) Mac 可能要關終端再開讓 PATH 重載 3) 確認 Claude Code 真的完全關閉再開 |
| Mac `which mcpvault` 找不到 | `npm config get prefix` 看 npm 全域 bin 位置，把該位置加到 `$PATH` |
| Windows bash 找不到 node | 新裝的 Node.js 可能不在 bash PATH 中，需要 `export PATH="/c/Program Files/nodejs:$PATH"` |
| iCloud 路徑找不到 vault | Mac 上 vault 在 `Documents/` 子資料夾內，不要少這層；Windows 路徑用 `\` 不是 `/` |
| Obsidian 開不到 vault | 用「Open folder as vault」指向 `[vault名稱]/` 整個資料夾，不是 `.obsidian/` 子資料夾 |
| Windows iCloud 同步很慢 | 在系統匣 iCloud 圖示確認同步進度；大量檔案首次同步可能要等幾分鐘到幾小時 |
| 多裝置編輯衝突，出現 `xxx 2.md` | 手動合併兩個檔的內容後刪除冗餘檔；避免同時在兩台裝置編輯同一篇 |
| `.obsidian/workspace.json` 一直衝突 | Obsidian → 設定 → 一般 → 關閉「啟動時還原前一個工作區」 |
| 中文檔名同步後變亂碼 | iCloud 對中文檔名有時會 NFC/NFD 編碼不一致，盡量檔名用半形或穩定字元 |
| 寫到一半 vault 看不到變更 | iCloud 同步未即時；等系統匣 iCloud 圖示完成同步（綠勾） |
| Obsidian 需要裝外掛嗎？ | **不需要**。mcpvault 直接讀寫 vault 資料夾內的檔案，跟 Obsidian app 無關 |

---

## 跨平台同步注意事項

### ✅ 會透過 iCloud 自動同步的
- 所有 `.md` 筆記內容
- `CLAUDE.md` 班規檔
- `Attachments/` 內的圖片附件
- 各種模板與設定文件

### ❌ 不會自動同步、要每台機器分別設定的
- mcpvault 全域安裝（每台 `npm install -g` 一次）
- Claude Code 的 MCP 註冊（每台 `claude mcp add` 一次）
- Obsidian 桌面版本身
- Node.js / Claude Code CLI

### ⚠️ 容易踩雷的同步點
- `.obsidian/workspace.json` 多裝置衝突 → 關掉「還原工作區」
- iCloud 同步延遲 → 切裝置前確認系統匣同步圖示已完成（綠勾）
- 大量附件首次同步 → Windows 下可在檔案總管右鍵「保留在裝置上」強制下載
- 中文 NFC/NFD 編碼 → 檔名避免特殊字元穩定不出狀況

---

## 同步方案對照

本懶人包預設使用 **iCloud**，如果你要改用其他方案：

| 方案 | Vault 路徑大致位置 | 適合 |
|------|------------------|------|
| **iCloud**（本懶人包） | `iCloud~md~obsidian/` 容器內 | Mac + iPhone + Windows 使用者 |
| **Google Drive** | `G:\我的雲端硬碟\` 或 `~/Library/CloudStorage/GoogleDrive-*/` | 跨 Google 生態系 |
| **Obsidian Sync** | 任意位置，Obsidian 內設定（$4/月） | 不想依賴第三方雲端 |
| **OneDrive / Dropbox** | 對應同步資料夾 | 已有訂閱者 |
| **Syncthing**（自架） | 任意位置，P2P 同步 | 不信任雲端、有自架經驗 |

> 差別只在 vault 路徑不同，MCP 連接指令的後半段換成對應路徑即可。

---

## 更新紀錄

| 日期 | 版本 | 更新內容 |
|------|------|---------|
| 2026-06-17 | v1.0 | 初版。iCloud × Windows + Mac 雙裝置實測通過 |

---

## 相關連結

- [mcpvault GitHub](https://github.com/bitbonsai/mcpvault)
- [Obsidian 官網](https://obsidian.md)
- [iCloud for Windows 下載](https://support.apple.com/zh-tw/118279)
- [Claude Code 文件](https://docs.claude.com/claude-code)
- [本 repo 的懶人包 #04（接續設定指南）](./04-第二大腦設定指南-三層結構.md)

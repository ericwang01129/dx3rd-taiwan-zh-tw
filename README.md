# DX3 系統正體中文化 (dx3rd-taiwan-zh-tw)

為 Foundry VTT `dx3rd` 系統提供正體中文介面，並補完繁中版規則書收錄的全套症候群、異能、工作、裝備、D-Lois 資料庫。

## 模組資訊

| 項目 | 值 |
|---|---|
| ID | `dx3rd-taiwan-zh-tw` |
| 版本 | 1.0.0 |
| Foundry 相容版本 | v11 |
| 依賴系統 | [`dx3rd`](https://github.com/cosbjn/dx3rd) |
| 語言 | `zh-tw` 正體中文 |

## 內含資料庫

| Compendium | 數量 | 說明 |
|---|---|---|
| DX3 症候群 Syndromes | 16 | 全 16 種症候群（含 Renegade Being） |
| DX3 異能 Effects | 1665 | 含 18 個分類（17 症候群 + 一般），自動帶入級數計算公式 |
| DX3 工作 Works | 103 | 全套工作條目 |
| DX3 裝備 Equipment | 1482 | 武器、防具、載具、紋章、消耗品、聯繫等 12 類 |
| DX3 D-Lois | 206 | 含咒物污染者、封具、秘密武器、遺產繼承者全套 |

## 主要功能

### 1. 介面/詞條中文化
所有 `dx3rd` 系統字串透過 `lang/zh-tw.json` 翻譯為正體中文，並以 `styles/dx3-overed-ui.css` 微調 UI 字型與排版適配中文。

### 2. 異能等級自動計算
所有異能的 `system.attributes` 已依說明文預先填入級數公式，啟用 (`active.state = true`) 時自動套用至角色屬性：

| 屬性鍵 | 對應規則 | 範例 |
|---|---|---|
| `attack` | 攻擊力 +X | 光之弓 = `@level+2` |
| `dice` | 判定/攻擊判定骰數 ±X 顆 | 完全安裝 = `@level*3` |
| `add` | 達成值 +X | 攻擊程序 = `@level*2` |
| `critical` | 暴擊值 -X | 紫電一閃 = `-1` |
| `guard` | 格擋值 +X | 磁力結界 = `@level` |
| `dodge_dice` | 回避骰數 +X 顆 | 分析 = `@level` |
| `damage_roll` | 傷害 +XD | 全開 = `@level` |
| `armor` | 裝甲值 +X | 龍鱗 = `@level*10` |

`@level` 於計算時由系統替換為當前異能等級。涉及對象/友方的 buff/debuff 不在此清單內，需手動處理。

### 3. 使用次數自動追蹤
依規則書「次數」欄填入 `system.used`，可選的重置時機：

| 規則欄位 | `used.disable` | 行為 |
|---|---|---|
| `1/回合` | `round` | 每回合結束自動歸零 |
| `1/場景`、`LV/場景` | `battle` | 每場景結束自動歸零 |
| `1/劇本`、`LV/劇本` | `notCheck` | 系統無自動劇本重置；劇本開始時請手動重置 |
| `1/判定` | `roll` | 判定後自動歸零 |
| `LV/X` | `level=true` | max 隨等級增長 |

### 4. 自動 Icon
症候群、異能、裝備自動套用 [game-icons.net](https://game-icons.net/) 圖示（CC BY 3.0），透過 GitHub raw 直連，**無需下載至本機**。

異能 icon 採用「關鍵字優先 → 症候群預設」兩段式比對：例如名稱含「火/炎」用 flame，含「冰/凍」用 ice-cube；找不到關鍵字則回退到所屬症候群圖示。

## 安裝

### 方法一：Foundry VTT Manifest 直接安裝（推薦）

於 Foundry **設定 → 附加元件模組 → 安裝模組** 的「Manifest URL」欄位貼上以下連結後按安裝：

```
https://raw.githubusercontent.com/ericwang01129/dx3rd-taiwan-zh-tw/main/module.json
```

### 方法二：手動下載

1. 直接下載 zip：<https://github.com/ericwang01129/dx3rd-taiwan-zh-tw/archive/refs/heads/main.zip>
2. 解壓後將整個資料夾放入 `Data/modules/dx3rd-taiwan-zh-tw/`
3. 確保已安裝 [`dx3rd` 系統](https://github.com/cosbjn/dx3rd)
4. 於 Foundry 內 **設定 → 管理模組** 啟用本模組
5. 重啟世界後於 **Compendium 列表** 即可看到 5 個正體中文資料庫

## 資料夾結構

```
dx3rd-taiwan-zh-tw/
├── module.json              模組定義
├── lang/zh-tw.json          介面翻譯
├── styles/                  CSS 微調
├── packs/                   已編譯的 LevelDB 資料庫（Foundry 直接讀取）
│   ├── dx3-syndromes/
│   ├── dx3-effects/
│   ├── dx3-works/
│   ├── dx3-equipment/
│   └── dx3-dlois/
├── src/                     原始 JSON（每筆條目一檔，便於版本控管/編輯）
│   ├── syndromes/
│   ├── effects/
│   ├── works/
│   ├── equipment/
│   └── dlois/
└── DX3rd *.ods              來源規則整理試算表（資料來源真相）
```

修改 `src/` 後需重新打包成 LevelDB 才會在遊戲中生效（Foundry 啟動時讀取 `packs/`）。

## 資料來源

- 規則文字：DX3rd 繁中規則書、補充本（HRP / EAP / BCP / ICP / RWP / UAP / CEP / RUP / CRC / IAP）
- 試算表：`DX3rd 異能資料整合版1.5 (繁).xlsx.ods`、`DX3rd 一般舞台道具表ver3.04(繁).xlsx.ods`、`DX3rd D-Lois資料ver1.7.xlsx.ods`

## 第三方授權

- 圖示：[game-icons.net](https://game-icons.net/) 由 lorc、delapouite、skoll、caro-asercion、sbed、carl-olsen、john-colburn、zeromancer 等作者提供，授權 [CC BY 3.0](https://creativecommons.org/licenses/by/3.0/)
- 系統：[`dx3rd` 系統](https://github.com/cosbjn/dx3rd)（本模組為其資料補完，不重新散布系統本體）
- 規則：DX3rd 為 F.E.A.R. 版權，本模組僅整理玩家使用所需資料，不含完整規則文本

## 已知限制

1. **per-劇本 重置**：dx3rd 系統 `used.disable` 無「劇本」自動重置選項，所有 `X/劇本` 限制設為 `notCheck`，需在新劇本開始時手動清空 `state`。
2. **icon 連線需求**：圖示直連 GitHub raw，**離線無法顯示**；首次載入有 ~100ms 延遲，瀏覽器快取後即時。
3. **異能級數公式**：僅自動套用「自身/組合此異能」類自我加成；對象 buff/debuff（友方/敵方）需手動處理。
4. **Foundry v12+ 相容性**：目前僅驗證至 v11，更高版本未測試。

## 作者

- eric（Discord: `ericwang01129`）

## 回報問題

於本專案 issue 區回報，提供：
1. 異能/裝備名稱
2. 預期值 vs 實際值
3. 規則書出處頁碼

歡迎 PR 修正錯字或補完資料。

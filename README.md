# 業務行動銷售系統（通用示範版）

給進口貿易批發商展示用的業務手機系統示範。單一檔案 `index.html`，CSS／JS 全部內嵌、無外部依賴，直接用瀏覽器開啟即可（離線可用）。

涵蓋功能：預售紀錄查詢（分頁、負數差異、白名單可異動／唯讀訂單）、品項詳細、新增／修改／刪除預售、我的預售、異常回報（原因多選、圖片、CSV 匯出）、行情表（報價記錄、兩種 CSV 匯出）、收款作業（現金／支票／暫收／暫欠、已鎖定、有沖帳不可刪、兩段刪除確認）、登入錯誤鎖定。

所有公司、客戶、廠商、品牌、業務員、金額皆為虛構示範資料。

## 示範帳密

| 帳號 | 密碼 | 身分 |
|------|------|------|
| `demo` | `demo123` | 業務員 王小明（S01） |
| `demo2` | `demo123` | 業務員 李小華（S02） |
| `viewer` | `demo123` | 示範主管（非業務員，沒有「我的預售」） |

同一帳號連續輸錯 5 次會鎖定 15 分鐘（按「重置示範資料」可解除）。

## 切換產業

內建四組產業組合，預設為生鮮蔬果：

| key | 產業 | 單位 |
|-----|------|------|
| `produce` | 生鮮蔬果 | 箱 |
| `meat` | 冷凍肉品 | 件 |
| `seafood` | 冷凍海鮮 | 箱 |
| `wine` | 進口酒類 | 箱 |

切換方式（任選一種）：

- 登入頁，或登入後每頁底部的「示範產業」選單。
- 網址加上參數，例如 `index.html?industry=wine`，方便直接把某個產業的連結給客戶。

**切換時會重建該產業的示範資料**：localStorage 只保存「目前這一個產業」的資料，切到別的產業時會清掉目前資料、依新產業重新產生，所以每次切換都是乾淨的示範情境。登入狀態會保留。若網址參數指定的產業與目前相同，則不會重建。

## 新增產業組合

打開 `index.html`，在 `<script>` 開頭找到 `INDUSTRY_PRESETS`，複製任一組合並改 key 與內容即可，選單會自動出現新選項。每組結構：

- `name`、`icon`、`unit`：產業名稱、登入頁圖示、數量單位。
- `labels`：畫面欄位名稱（`product` 品名、`variety` 品種、`brand`／`markBrand` 品牌、`nation` 產地、`cls` 等級、`size` 規格、`container` 櫃號、`air` 批號、`supplier` 廠商、`unload`／`unloadOptions` 卸貨地、`transport`／`sea`／`airFreight`／`both`／`seaShort`／`airShort` 運輸方式、`transfer`／`notShipped`／`shipped` 轉出貨，以及各查詢欄位的 placeholder `productHint`／`varietyHint`／`brandHint`）。
- `reasons`：異常回報原因（6 個）。
- `data`：示範資料。`customers` 8 家、`suppliers` 3 家（含英文品牌）、`countries` 5 個、`products` 6 項（每項 2 個 `varieties` [中文, 英文] 與 4 個 `specs`）、`grades` 3 個、`markVendors` 4 家、`defects` 3 筆（`reason`／`tags` 填 `reasons` 的索引）。

陣列長度請比照現有組合：訂單、預售、收款等示範情境是各產業共用的，靠索引對應到這些陣列，數量不足會出錯。

## 重置示範資料

- 登入頁或每頁底部按「重置示範資料」（重建目前產業的資料並解除登入鎖定）。
- 或在 DevTools Console 執行 `localStorage.clear()` 後重新整理。

localStorage key 前綴為 `salesphone_generic_v1`（`_data`、`_session`、`_throttle`、`_lastquery`、`_industry`），不會與其他示範版衝突。

# SweetStack 甜點棧｜C2C 電商平台

以 **ASP.NET Core MVC、C#、SQL Server 與 Vue.js** 開發的 C2C 電商平台，模擬買家購物、訂單結帳、第三方付款，以及賣家營運管理等情境。

本專案為 C# 全端工程師培訓課程之團隊專題。我主要負責購物車、訂單結帳、第三方金流、優惠券驗證，以及賣家後台數據中心等功能，著重於交易流程整合、資料驗證與使用者操作體驗。

## Tech Stack

| 類別 | 使用技術 |
|---|---|
| Backend | C#、ASP.NET Core MVC、Web API |
| Frontend | Razor、HTML、CSS、JavaScript、Vue.js |
| Database | SQL Server、Entity Framework Core、LINQ |
| State Management | ASP.NET Core Session、Cookie |
| Payment Integration | NewebPay 藍新金流 |
| Real-time Communication | ASP.NET Core SignalR |
| Version Control | Git、GitHub |

## 核心交易流程

買家選購商品 → Session 購物車 → 填寫收件資訊 → 優惠券與庫存驗證 → 建立訂單 → 選擇付款方式 → 訂單狀態更新 → 訂單通知

系統支援貨到付款與第三方線上金流，依付款方式執行不同的訂單處理流程。

## My Contributions｜個人負責模組

### 1. Shopping Cart｜購物車管理

**技術：ASP.NET Core Session、Fetch API、Vue.js**

- 使用 Session 儲存購物車資料，透過 JSON 序列化與反序列化管理購物車狀態。
- 建立加入商品、調整數量、移除商品與取得購物車資料的 Controller Action。
- 使用 Vue.js Mixin 封裝共用購物車邏輯，計算商品小計、購物車數量與結帳總金額。
- 透過自訂 `update-cart` 事件通知不同購物車介面更新。
- 使用 500ms debounce 控制數量調整時的 API 呼叫頻率，改善操作體驗。

**開發重點：** Session 資料管理、前後端狀態同步與共用元件邏輯。

### 2. Order & Payment｜訂單與第三方金流

**技術：ASP.NET Core Web API、EF Core、NewebPay、AES、SHA256**

- 串接藍新金流，依金流規格產生交易參數及加密資料。
- 建立訂單與訂單明細，處理商品數量、單價、運費及優惠券折扣。
- 於建立訂單前檢查商品庫存，避免明顯超出庫存的交易。
- 使用 EF Core Database Transaction 處理訂單建立，以及相關庫存更新。
- 透過付款 Callback 接收交易結果，處理付款成功、失敗與訂單狀態更新。
- 加入訂單狀態判斷，降低重複付款通知造成重複處理的風險。

**開發重點：** 第三方 API 串接、交易流程、訂單狀態與資料一致性。

### 3. Coupon Validation｜優惠券驗證

**技術：ASP.NET Core Web API、EF Core、Cookie**

- 建立優惠券驗證 API，檢查優惠碼是否存在、是否啟用及是否過期。
- 根據購物車金額檢查最低消費門檻。
- 計算折扣金額，避免前端顯示的折扣超過符合條件的商品金額。
- 使用 Cookie 暫存套用的優惠碼。
- 在建立訂單時於後端重新驗證優惠券，避免直接信任前端傳入的折扣金額。

**開發重點：** 業務規則驗證、後端資料檢查與結帳流程整合。

### 4. Seller Dashboard｜賣家營運數據中心

**技術：Entity Framework Core、LINQ、ASP.NET Core MVC**

將訂單、商品及庫存資料整理為賣家可查看的營運資訊，包括：

- 今日與本月營收
- 本月訂單數、待出貨訂單數
- 近七日訂單數量趨勢
- 商品分類銷售額
- 熱銷商品 Top 5
- 低庫存商品提醒
- 在架商品數及追蹤者數量

透過 LINQ 條件查詢、分組與聚合計算，依登入賣家篩選資料並傳入 Dashboard ViewModel。

**開發重點：** 資料查詢、統計口徑設計與營運資訊呈現。


## Project Structure｜主要程式位置

| 檔案 | 功能 |
|---|---|
| `Controllers/CartController.cs` | Session 購物車管理 |
| `Controllers/CheckoutController.cs` | 結帳頁面及訂單通知 |
| `Controllers/api/PayApiController.cs` | 訂單建立與金流資料產生 |
| `Controllers/PayController.cs` | 金流 Callback 與付款結果處理 |
| `Controllers/api/CouponApiController.cs` | 優惠券驗證 |
| `Areas/Seller/Controllers/SellerDashboardController.cs` | 賣家營運數據查詢 |

## Local Setup｜本機執行

開發環境：

- Visual Studio
- .NET SDK（版本待補）
- SQL Server

基本步驟：

1. Clone Repository，開啟 `Bake.sln`。
2. 建立本機 SQL Server 資料庫並完成資料初始化。
3. 設定本機 `appsettings.json`，包含資料庫連線及必要的金流測試參數。
4. 還原 NuGet 套件並啟動專案。

本 Repository 不提供正式資料庫憑證及金流密鑰。目前尚未提供完整測試資料與自動初始化流程，Clone 後可能需要額外設定才能執行。

## Future Improvements｜未來改善方向

- 加強付款 Callback 的簽章驗證與重複通知處理。
- 改善購物車快速連續調整數量時的狀態同步。
- 強化庫存並行更新及資料一致性。
- 統一優惠券驗證與最終折扣計算規則。
- 增加交易流程的自動化測試。
- 提供測試資料及更完整的本機啟動說明。

## Team Project Statement｜團隊專案聲明

本專案為 C# 全端工程師培訓課程之團隊共同開發成果。

此 Repository 為個人作品集整理版本，主要展示本人參與開發的功能與技術實作。其他系統模組由團隊成員共同完成，並非全部由本人獨立開發。

原始團隊 Repository：待取得團隊同意後補充。

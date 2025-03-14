# 即食予 DIDA Dream

「只要全球減少 1/4 的食物浪費，就可以餵飽每個人」，我們由此發想「即食予」企劃，

透過整合即期品的 [LINE 官方帳號](https://page.line.me/?accountId=195dceee)，實現剩食再分配的目標。

我們使用全台超商 API 及在地商家註冊系統取得即期品資訊。透過具創意的 You Are What You Eat 企劃，吸引客戶加入，
並藉此取得用戶偏好、精準推播，提升使用者黏著度。
同時，結合 LINE 禮物搭建線上捐贈平台，讓剩食有效地送到需要的人手中。
擴大品牌影響力後，不僅能提升即期品銷量，也讓理念被更多人看見，進而響應捐贈，以剩食為燈，點亮需要幫助的家庭。

完整企劃介紹：https://prezi.com/view/tweaIMQvrEdc4rFSVIEQ/

# 技術架構圖
![](https://i.imgur.com/oWAvGFq.png)

# Web File Usage

- **心理測驗** `psycho_test`
    - `start.php`: 心理測驗之起始頁面
    - `test_page1.php`: 心理測驗第一題的頁面
    - `test_page2.php`: 心理測驗第二題的頁面
    - `test_page3.php`: 心理測驗第三題的頁面
    - `test_page4.php`: 心理測驗第四題的頁面
    - `test_page5.php`: 心理測驗第五題的頁面
    - `test_page6.php`: 心理測驗第六題的頁面
    - `final.php`: 顯示測驗結果的頁面（設有一鍵分享功能）
    - `test.php`: 處理各題問題之回答，並存到 database

- **個人資料＆商家資料** `edit_user.php`
    - `edit_shop.php`: 修改商家檔案
    - `edit_user_php.php`: 修改個人檔案

- **商品檔案** `shop.php`
    - `add_meal.php`: 新增商品到商品檔案
    - `delete_meal.php`: 將商品從商品檔案中移除
    - `shop_search.php`: 在商品檔案頁面中搜尋特定商品
    - `edit_meal.php`: 更改商品資訊

- **我的即期品** `leftovers_page.php`
    - `clear_meal.php`:  一鍵清除所有即期品
    - `leftovers_search.php`: 在我的即期品頁面中搜尋特定即期品
    - `update_quantity.php`: 更改即期品數量（按 + , - 按鈕的時候）

- **搜尋** `search_page.php`
    - `search.php`: 執行使用者設定之設定條件，篩選出符合條件的店家
    - `select.php`: 顯示所選店家的所有即期品

- **Line Bot**
    - `privacy_doc.html`: 參與即時予計畫之隱私權條款
    - `buygift.php`: 購買即時券的流程展示（網頁實作之 prototype）

- **註冊** `register_page.php` → 使用者、 `register_shop.html` → 店家
    - `register.php`: 將新註冊之使用者資訊存到 users 的資料庫
    - `register_shop.php`: 將新註冊之店家資訊存到 shops 的資料庫
    - `register_page1.php`: 讓使用者選擇欲註冊之身份（後來沒用了）
    - `register1.php`: 重新導向至欲註冊身份的填寫頁面

- **其他**
    - `redirect.php`: 根據網址 flag 將使用者重新導向至該頁面，同時將使用者 id 傳給 set_uid.php
    - `connect.php`: 連至 phpMyAdmin databas（包含資料庫 IP 等等）
    - `set_uid.php`: 將傳進來的 user id 存進 session
    - `record.php`: 展示集點卡頁面（設有一鍵分享功能）
    - `update.php`: 用來更新 s_id 讓他在 shops 跟 meals 的 database 都一致（用了一次就不需要了）

# LINEBot Usage
## 技術介紹
1. 客製化主動推播：在 server 內建立一條 thread，並設定根據資料庫的資料定期推播消息給使用者
2. 圖文選單：利用 Rich Menu API 來實現多個圖文選單切換，並且可以根據使用者身分設定每個人的選單形式
3. LINE LIFF：利用 LIFF API 讓網頁端可以直接獲取個人資料如 LINE ID 等等，可以省去 redirect 到網站的成本

## 使用者選單
### 主選單
![](https://i.imgur.com/2RUxQmj.png)
- 查詢即期品：根據使用者偏好、位置，客製化顯示出適合他的商家以及即期品品項
- 優惠地圖：預計可連結至 LINE SPOT 的優惠地圖
- 推薦給好友：可一鍵分享即食予官方帳號給好友
- 資料修改：連結至 edit_user.php 的頁面進行個人資料修改
- 聯絡我們：透過 flex message 顯示聯絡訊息

### 即食予
![](https://i.imgur.com/CtNq6bJ.png)
- 發送即食卷：連結到 LINE 禮物捐贈即食卷給 requesters，透過後端資料庫配對給需要的人
- 集點卡：連結到 record.php 顯示集點卡
- 支持我們：預計可連結至 LINE PAY
- 了解計畫：透過 flex message 大致介紹即時予計畫
   
## 商家選單
### 主選單
![](https://i.imgur.com/TIQKDbO.png)
- 商品修改：連結至 edit_meal.php 修改商品
- 加入會員：加入會員可享有會員回饋
- 資料修改：可一鍵分享即食予官方帳號給好友
- 聯絡我們：透過 flex message 顯示聯絡訊息

### 其他功能
![](https://i.imgur.com/YBdDWdv.png)
- 查詢即期品：根據使用者偏好、位置，客製化顯示出適合他的商家以及即期品品項
- 優惠地圖：預計可連結至 LINE SPOT 的優惠地圖
- 推薦給好友：可一鍵分享即食予官方帳號給好友

### 即食予
![](https://i.imgur.com/MASRRCL.png)
- 參與計畫：小商家加入即時予計畫的管道
- 了解計畫：透過 flex message 大致介紹即時予計畫
- 解鎖會員功能

## Requesters 選單
### 主選單
![](https://i.imgur.com/2RUxQmj.png)
- 查詢即期品：根據使用者偏好、位置，客製化顯示出適合他的商家以及即期品品項
- 優惠地圖：預計可連結至 LINE SPOT 的優惠地圖
- 推薦給好友：可一鍵分享即食予官方帳號給好友
- 資料修改：連結至 edit_user.php 的頁面進行個人資料修改
- 聯絡我們：透過 flex message 顯示聯絡訊息

### 即食予
![](https://i.imgur.com/B9kwZfw.png)
- 收取即食卷：透過後端資料庫配對收取捐贈者捐贈的即食卷
- 了解計畫：透過 flex message 大致介紹即時予計畫

# Netflix 片單查詢機器人

<br>

[![觀看操作影片](https://img.youtube.com/vi/kF8BJgmL-qE/0.jpg)](https://www.youtube.com/watch?v=kF8BJgmL-qE)

[觀看操作影片](https://www.youtube.com/watch?v=kF8BJgmL-qE)

<br>

## 主要功能

使用者在 LINE Bot 中輸入指定指令，系統即可回傳串流平台 Netflix 的新片清單、熱門排行等資訊。

此外，還能夠將指定內容分享給 LINE 好友，與朋友一同討論，促進交流。

<br>

## 立即體驗

加入 LINE Bot：掃描以下 QR code，或<a href="https://line.me/R/ti/p/@673rabpy">點擊這裡</a>

<img src="https://i.imgur.com/86wnboP.png" alt="Netflix 片單查詢機器人" width="20%">

<br>

## 專案發想

平時在使用串流平台 Netflix 時，常會關注「最近有什麼新片上線、熱門排行有哪些?」。

然而，如果要查看這些資訊，都會需要開啟指定網頁，累積下來就會花費不少時間。

「是否有更快速的方式來獲取新片清單、熱門排行?」即為此次專案的發想起點。

<br>

## 解決方案

使用 Python 的爬蟲套件(requests, BeautifulSoup, Playwright)爬取單頁、多頁的資料，再結合 LINE BOT SDK ，以達到本次專案預期功能。

之所以使用 LINE Bot 作為本次專案介面，是根據 <a href="https://datareportal.com/reports/digital-2023-taiwan?_trms=ac920f3ad09a7a33.1693645815116">DIGITAL 2023: TAIWAN</a> 指出 LINE 為台灣占主導地位的社群媒體平台。

透過該介面能夠接觸到更多的潛在使用者。

此外，LINE Bot 中的呈現方式 (如：泡泡、Carousel) 可以清楚列出文字內容並搭配圖片，有利於使用者進行評估、選擇。

<br>

## 開發過程紀錄

1. 規劃預期效果、使用者介面、整體使用流程

2. 觀察 Netflix 頁面：需要爬取甚麼資料、所需資料是否橫跨多個頁面

3. 進行爬蟲，取出所有資料

4. 整理爬取下的資料

5. 使用 Python Flask 串接 LINE Bot ，嘗試匯出所需內容

6. 確認成功匯出後，打包成函式

7. 問題克服

8. 使用者反饋

9. 後續微調

<br>

## 開發期間遇到的問題

### 1. 回傳的內容要放入哪些項目

【問題說明】從開始開發時，便從使用者的角度思考「我在使用時會希望看到甚麼資訊呢?」，使用者需要哪些資訊，才利於做出決定(選定要看哪一齣節目、哪一部電影)。然而，如果放上太多的資訊，反而無助使用者作出抉擇，也不利於呈現在 LINE 上。本次專案僅由我一人負責，故無法確定哪些項目適合留下。

【解決方法】透過詢問周邊朋友的觀影習慣，問題如「平時在挑選 Netflix 節目、電影時，會從哪些項目判斷? 出版年份? 類型? 演員? 簡介?」。最後定案為保留「片名、出版年份、網址、上線日期、類型、演員、簡介」等資訊項目，並且針對「簡介」製作了兩個版本，供使用者自行選擇當下是否需要簡介說明。

### 2. 排名資料頁面為全英文，不能直接使用

【問題說明】熱門排名資料是從 Neflix Tudum 上爬取，然該目前該網站只提供英文版，無法直接抓取頁面上的資料進行使用。

【解決方法】研究 Tudum 官網的 HTML 後，發現其中包含了影片的 ID 號碼，可透過該號碼再連結至另一頁面進行資料爬取。

<br>

## 爬取頁面

### 1. <a href="https://about.netflix.com/zh_tw/new-to-watch">Netflix 新片</a>

抓取資訊：所有影片的ID及上架日期。

<img src="https://i.imgur.com/Yf4yRQR.png" alt="Netflix 新片頁面" width="80%">

### 2. <a href="https://www.netflix.com/tw/title/81942643">Netflix 影片介紹</a>

抓取資訊：影片名稱、片長、影片類別、影片簡介、主演名單、創作者名單、詳細類型、影片性質等。

<img src="https://i.imgur.com/zfeoL5e.jpeg" alt="Netflix 影片頁面" width="80%">

### 3. <a href="https://www.netflix.com/tudum/top10/taiwan">Netflix Top 10 電影</a>

抓取資訊：影片ID。

<img src="https://i.imgur.com/CU7uo7k.png" alt="Netflix Top 10 電影" width="80%">

### 4. <a href="https://www.netflix.com/tudum/top10/taiwan/tv">Netflix Top 10 節目</a>

抓取資訊：影片ID。

<img src="https://i.imgur.com/J3qnw33.png" alt="Netflix Top 10 節目" width="80%">

<br>

## 使用方式

### 新片 - 電影

1. 輸入 ---> 文字【Netflix 片單查詢】<br>
   回傳 ---> 泡泡【Netflix 片單查詢】<br>
   
   <img src="https://i.imgur.com/PDEd3en.jpeg" alt="泡泡【Netflix 片單查詢】" style="max-width: 100%; height: auto;">

2. 點擊 ---> 泡泡【Netflix 片單查詢】 - 新片 電影<br>
   帶入 ---> 文字【Netflix 新片(電影-不含簡介) 1-10】<br>
   回傳 ---> 泡泡【Netflix 新片(電影-不含簡介) 1-10】<br>
             (如果超過 10 部，最後會顯示下一頁。)
   
   <img src="https://i.imgur.com/9vNkjIM.jpeg" alt="泡泡【Netflix 新片(電影-不含簡介) 1-10】" style="max-width: 100%; height: auto;">  
   <img src="https://i.imgur.com/yHlO9NB.jpeg" alt="泡泡【Netflix 新片(電影-不含簡介) 1-10 下一頁】" style="max-width: 100%; height: auto;">

4. 點擊 ---> 泡泡【Netflix 新片(電影-不含簡介) 1-10】 - 預告<br>
   跳出 ---> 影片對應連結<br>
   
   <img src="https://i.imgur.com/qxf3Bi3.jpeg" alt="影片預告連結" width="20%">

5. 點擊 ---> 泡泡【Netflix 新片(電影-不含簡介) 1-10】 - 下一頁<br>
   帶入 ---> 文字【Netflix 新片(電影-不含簡介) 11-20】<br>
   回傳 ---> 泡泡【Netflix 新片(電影-不含簡介) 11-20】<br>
            (最後會顯示上一頁；如果超過 20 部，最後會顯示下一頁，依此類推。)
   
   <img src="https://i.imgur.com/HnxWxOT.jpeg" alt="泡泡【Netflix 新片(電影-不含簡介) 11-20】" style="max-width: 100%; height: auto;">
   <img src="https://i.imgur.com/uDCHGxF.jpeg" alt="泡泡【Netflix 新片(電影-不含簡介) 11-20 上一頁】" style="max-width: 100%; height: auto;">

7. 點擊 ---> 泡泡【Netflix 新片(電影-不含簡介) 11-20】 - 切換至簡介版本<br>
   帶入 ---> 文字【Netflix 新片(電影-含簡介) 11-20】<br>
   回傳 ---> 泡泡【Netflix 新片(電影-含簡介) 11-20】<br>
   
   <img src="https://i.imgur.com/v1dYGMa.jpeg" alt="泡泡【Netflix 新片(電影-含簡介) 11-20】" style="max-width: 100%; height: auto;">  

### 新片 - 節目

1. 輸入 ---> 文字【Netflix 片單查詢】<br>
   回傳 ---> 泡泡【Netflix 片單查詢】<br>
   
   <img src="https://i.imgur.com/OcscaxV.jpeg" alt="泡泡【Netflix 片單查詢】" style="max-width: 100%; height: auto;">

2. 點擊 ---> 泡泡【Netflix 片單查詢】 - 新片 節目<br>
   帶入 ---> 文字【Netflix 新片(節目-不含簡介) 1-10】<br>
   回傳 ---> 泡泡【Netflix 新片(節目-不含簡介) 1-10】<br>
             (如果超過 10 部，最後會顯示下一頁。)
   
   <img src="https://i.imgur.com/xK8X6yl.jpeg" alt="泡泡【Netflix 新片(節目-不含簡介) 1-10】" style="max-width: 100%; height: auto;">  
   <img src="https://i.imgur.com/2gyCCyE.jpeg" alt="泡泡【Netflix 新片(節目-不含簡介) 1-10 下一頁】" style="max-width: 100%; height: auto;">

4. 點擊 ---> 泡泡【Netflix 新片(節目-不含簡介) 1-10】 - 預告<br>
   跳出 ---> 影片對應連結<br>
   
   <img src="https://i.imgur.com/ry2Z7kO.jpeg" alt="影片預告連結" width="20%">

5. 點擊 ---> 泡泡【Netflix 新片(節目-不含簡介) 1-10】 - 下一頁<br>
   帶入 ---> 文字【Netflix 新片(節目-不含簡介) 11-20】<br>
   回傳 ---> 泡泡【Netflix 新片(節目-不含簡介) 11-20】<br>
            (最後會顯示上一頁；如果超過 20 部，最後會顯示下一頁，依此類推。)
            
   <img src="https://i.imgur.com/jM36uDK.jpeg" alt="泡泡【Netflix 新片(節目-不含簡介) 11-20】" style="max-width: 100%; height: auto;">  
   <img src="https://i.imgur.com/2ifPLQr.jpeg" alt="泡泡【Netflix 新片(節目-不含簡介) 11-20 上一頁】" style="max-width: 100%; height: auto;">

6. 點擊 ---> 泡泡【Netflix 新片(節目-不含簡介) 11-20】 - 切換至簡介版本<br>
   帶入 ---> 文字【Netflix 新片(節目-含簡介) 11-20】<br>
   回傳 ---> 泡泡【Netflix 新片(節目-含簡介) 11-20】<br>
   
   <img src="https://i.imgur.com/c4IDAfN.jpeg" alt="泡泡【Netflix 新片(節目-含簡介) 11-20】" style="max-width: 100%; height: auto;">  

### Top 10 - 電影

1. 輸入 ---> 文字【Netflix 片單查詢】<br>
   回傳 ---> 泡泡【Netflix 片單查詢】<br>
   
   <img src="https://i.imgur.com/dyzL1lI.jpeg" alt="泡泡【Netflix 片單查詢】" style="max-width: 100%; height: auto;">

2. 點擊 ---> 泡泡【Netflix 片單查詢】 - Top 10 電影<br>
   帶入 ---> 文字【Netflix Top 10 (電影-不含簡介)】<br>
   回傳 ---> 泡泡【Netflix Top 10 (電影-不含簡介)】<br>
   
   <img src="https://i.imgur.com/dt2QRAr.jpeg" alt="泡泡【Netflix Top 10 (電影-不含簡介)】" style="max-width: 100%; height: auto;">  
   <img src="https://i.imgur.com/TI4HQTk.jpeg" alt="泡泡【Netflix Top 10 (電影-不含簡介)】" style="max-width: 100%; height: auto;">  

3. 點擊 ---> 泡泡【Netflix Top 10 (電影-不含簡介)】 - 預告<br>
   跳出 ---> 影片對應連結<br>
   
   <img src="https://i.imgur.com/kyYR1g0.jpeg" alt="影片預告連結" width="20%">

4. 點擊 ---> 泡泡【Netflix Top 10 (電影-不含簡介)】 - 切換至簡介版本<br>
   帶入 ---> 文字【Netflix Top 10 (電影-含簡介)】<br>
   回傳 ---> 泡泡【Netflix Top 10 (電影-含簡介)】<br>
   
   <img src="https://i.imgur.com/hxUIOqO.jpeg" alt="泡泡【Netflix Top 10 (電影-含簡介)】" style="max-width: 100%; height: auto;">

### Top 10 - 節目

1. 輸入 ---> 文字【Netflix 片單查詢】<br>
   回傳 ---> 泡泡【Netflix 片單查詢】<br>
   
   <img src="https://i.imgur.com/hIdGOEv.jpeg" alt="泡泡【Netflix 片單查詢】" style="max-width: 100%; height: auto;">

2. 點擊 ---> 泡泡【Netflix 片單查詢】 - Top 10 節目<br>
   帶入 ---> 文字【Netflix Top 10 (節目-不含簡介)】<br>
   回傳 ---> 泡泡【Netflix Top 10 (節目-不含簡介)】<br>
   
   <img src="https://i.imgur.com/2G9r0k7.jpeg" alt="泡泡【Netflix Top 10 (節目-不含簡介)】" style="max-width: 100%; height: auto;">
   <img src="https://i.imgur.com/jwW1ArE.jpeg" alt="泡泡【Netflix Top 10 (節目-不含簡介)】" style="max-width: 100%; height: auto;">

3. 點擊 ---> 泡泡【Netflix Top 10 (節目-不含簡介)】 - 預告<br>
   跳出 ---> 影片對應連結<br>
   
   <img src="https://i.imgur.com/Q4zTNb7.jpeg" alt="影片預告連結" width="20%">

4. 點擊 ---> 泡泡【Netflix Top 10 (節目-不含簡介)】 - 切換至簡介版本<br>
   帶入 ---> 文字【Netflix Top 10 (節目-含簡介)】<br>
   回傳 ---> 泡泡【Netflix Top 10 (節目-含簡介)】<br>
   
   <img src="https://i.imgur.com/gmSCOoO.jpeg" alt="泡泡【Netflix Top 10 (節目-含簡介)】" style="max-width: 100%; height: auto;">

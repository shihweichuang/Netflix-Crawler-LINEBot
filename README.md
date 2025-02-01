# 主要功能

使用者在 LINE Bot 中輸入指定指令，系統即可回傳 Netflix 新片清單、熱門排行等資訊。

此外，還能夠將指定內容分享給 LINE 好友，與朋友一同討論，促進交流。

<br>

# 專案發想

平時在使用串流平台 Netflix 時，會關注最近有什麼新片上線、熱門排行有哪些。

然而，如果要查看這些資訊，都會需要開啟指定網頁，會花費一點時間。

「是否有更快速的方式來獲取新片清單、熱門排行?」即為此次專案的發想起點。

<br>

# 解決方案

使用 Python 的爬蟲套件(requests, BeautifulSoup, Playwright)爬取單頁、多頁的資料，再結合 LINE BOT SDK ，以達到本次專案預期功能。

之所以使用 LINE Bot 作為本次專案介面，是根據 <a href="https://datareportal.com/reports/digital-2023-taiwan?_trms=ac920f3ad09a7a33.1693645815116">DIGITAL 2023: TAIWAN</a> 指出 LINE 為台灣占主導地位的社群媒體平台。

<br>

# 開發過程紀錄

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

# 開發期間遇到的問題

## 1. 回傳的內容要放入哪些項目

從開始開發時，便從使用者的角度思考「我在使用時會希望看到甚麼資訊呢?」，使用者需要哪些資訊，才利於做出決定(選定要看哪一齣節目、哪一部電影)。然而，如果放上太多的資訊，反而無助使用者作出抉擇，也不利於呈現在 LINE 上。

本次專案僅由我一人負責，故無法確定哪些項目適合留下。

### 解決方法

透過詢問周邊朋友的觀影習慣，問題如「平時在挑選 Netflix 節目、電影時，會從哪些項目判斷? 出版年份? 類型? 演員? 簡介?」

最後定案為保留「片名、出版年份、網址、上線日期、類型、演員、簡介」等資訊項目，並且針對「簡介」製作了兩個版本，供使用者自行選擇當下是否需要簡介說明。

## 2. 爬取資料的時間較長

由於爬取資料的時間較長，可能會讓使用者等得不耐煩，甚至會覺得是系統當機。

### 解決方法

每天只會執行一次爬取資料的過程，並將爬取下來的資料另存為JSON檔案，並於檔案名稱中加上當日日期。

在往後的每次執行，都會先判斷是否含有當日日期的檔案名稱。若有，則讀取該檔案；若無，則進行資料爬取。


## 3. 呈現的資訊過長

從 Netflix 官網上爬取的資料長度過長，以至於無法在 LINE Bot 中成功回傳。

### 解決方法

將原先的檔案切分為適當的長度。此外，雖然能夠一次回傳多個檔案內容，但是可讀性不高，故改為一次僅回傳一個檔案的內容，並在最後附上【換頁按鈕】，供使用者自行決定是否閱讀更多內容。

<br>

# 使用方式

## 新片 - 電影

1. 輸入 ---> Netflix 片單查詢<br>
   回傳 ---> Netflix 片單查詢
<img src="https://i.imgur.com/dALuhfA.jpeg" alt="Bubble【Netflix 片單查詢】" width="278" height="420">

2. 點擊 ---> Netflix 片單查詢 - 新片 電影<br>
   帶入 ---> Netflix 新片(電影-不含簡介) 1-10<br>
   回傳 ---> Netflix 新片(電影-不含簡介) 1-10<br>
             (如果超過 10 部，最後會顯示下一頁。)
<div style="text-align: right;">
   <img src="https://i.imgur.com/UEc5yqt.jpeg" alt="Bubble【Netflix 新片(電影-不含簡介) 1-10】" width="278" height="480">  
   <img src="https://i.imgur.com/k5rS6IG.jpeg" alt="Bubble【Netflix 新片(電影-不含簡介) 1-10 下一頁】" width="278" height="480">
</div>
<br>

3. 點擊 ---> Netflix 新片(電影-不含簡介) 1-10 預告<br>
   跳出 ---> 影片對應連結
<img src="https://i.imgur.com/yADsSU6.jpeg" alt="影片預告連結" width="278" height="560">

4. 點擊 ---> Netflix 新片(電影-不含簡介) 1-10 下一頁<br>
   帶入 ---> Netflix 新片(電影-不含簡介) 11-20<br>
   回傳 ---> Netflix 新片(電影-不含簡介) 11-20<br>
            (最後會顯示上一頁；如果超過 20 部，最後會顯示下一頁，依此類推。)
<div style="text-align: right;">
   <img src="https://i.imgur.com/VbXBazg.jpeg" alt="Bubble【Netflix 新片(電影-不含簡介) 11-20】" width="278" height="480">  
   <img src="https://i.imgur.com/aOwribP.jpeg" alt="Bubble【Netflix 新片(電影-不含簡介) 11-20 上一頁】" width="278" height="480">
</div>
<br>

5. 點擊 ---> Netflix 新片(電影-不含簡介) 11-20 切換至簡介版本<br>
   帶入 ---> Netflix 新片(電影-含簡介) 11-20<br>
   回傳 ---> Netflix 新片(電影-含簡介) 11-20<br>
<div style="text-align: right;">
   <img src="https://i.imgur.com/DWJhbRI.jpeg" alt="Bubble【Netflix 新片(電影-含簡介) 11-20】" width="278" height="540">  
</div>

## 新片 - 節目

1. 輸入 ---> Netflix 片單查詢<br>
   回傳 ---> Netflix 片單查詢
<img src="https://i.imgur.com/dALuhfA.jpeg" alt="Bubble【Netflix 片單查詢】" width="278" height="420">

2. 點擊 ---> Netflix 片單查詢 - 新片 節目<br>
   帶入 ---> Netflix 新片(節目-不含簡介) 1-10<br>
   回傳 ---> Netflix 新片(節目-不含簡介) 1-10<br>
             (如果超過 10 部，最後會顯示下一頁。)
<div style="text-align: right;">
   <img src="https://i.imgur.com/9Hif4Yi.jpeg" alt="Bubble【Netflix 新片(節目-不含簡介) 1-10】" width="278" height="480">  
   <img src="https://i.imgur.com/a0THv65.jpeg" alt="Bubble【Netflix 新片(節目-不含簡介) 1-10 下一頁】" width="278" height="480">
</div>
<br>

3. 點擊 ---> Netflix 新片(節目-不含簡介) 1-10 預告<br>
   跳出 ---> 影片對應連結
<img src="https://i.imgur.com/R866KBW.jpeg" alt="影片預告連結" width="278" height="560">

4. 點擊 ---> Netflix 新片(節目-不含簡介) 1-10 下一頁<br>
   帶入 ---> Netflix 新片(節目-不含簡介) 11-20<br>
   回傳 ---> Netflix 新片(節目-不含簡介) 11-20<br>
            (最後會顯示上一頁；如果超過 20 部，最後會顯示下一頁，依此類推。)
<div style="text-align: right;">
   <img src="https://i.imgur.com/Dh6yPl6.jpeg" alt="Bubble【Netflix 新片(節目-不含簡介) 11-20】" width="278" height="480">  
   <img src="https://i.imgur.com/Lhzw8IF.jpeg" alt="Bubble【Netflix 新片(節目-不含簡介) 11-20 上一頁】" width="278" height="480">
</div>
<br>

5. 點擊 ---> Netflix 新片(節目-不含簡介) 11-20 切換至簡介版本<br>
   帶入 ---> Netflix 新片(節目-含簡介) 11-20<br>
   回傳 ---> Netflix 新片(節目-含簡介) 11-20<br>
<div style="text-align: right;">
   <img src="https://i.imgur.com/q2tXVwq.jpeg" alt="Bubble【Netflix 新片(節目-含簡介) 11-20】" width="278" height="540">  
</div>

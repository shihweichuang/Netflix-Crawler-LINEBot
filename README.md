# Netflix crawler 功能說明

使用者可以直接在 LINE Bot 中輸入指定指令，系統即可回傳 Netflix 新片清單、熱門排行。

此外，還能夠將指定內容分享給 LINE 好友，與朋友一同討論，促進交流。

<br>

# 專案發想

平時在看 Netflix 時，會關注最近有什麼新片上線、熱門排行有哪些。

然而，如果要查看這些資訊，都會需要開啟指定網頁，會花費一點時間。

「是否有更快速的方式來獲取新片清單、熱門排行?」即為此次專案的發想起點。

<br>

# 解決方案

使用 Python 的爬蟲套件(requests, BeautifulSoup, Selenium)爬取單頁、多頁的資料，再結合 LINE BOT SDK ，以達到本次專案預期功能。

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

最後定案為保留「片名、出版年份、網址、上線日期、類型、簡介」等資訊項目，並且針對「簡介」製作了兩個版本，供使用者自行選擇當下是否需要簡介說明。

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

## 新片清單

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">
<br>
2. 使用者點擊 ---> Bubble【查詢】中的按鈕【新片】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片】<br>
   系統回傳 ---> Bubble【新片清單】
<img src="https://i.imgur.com/TVEf4Rn.jpg" alt="Bubble【新片清單】" width="278" height="335">

## 新片(含影片簡介)

1. 使用者點擊 ---> Bubble【新片清單】中的按鈕【含影片簡介】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(含簡介) --- 1-20】<br>
   系統回傳 ---> 文字【Netflix 新片(含簡介) 1-20】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/L3ORxEU.jpg" alt="文字【Netflix 新片(含簡介) 1-20】" width="278" height="446">
<img src="https://i.imgur.com/xu6puEJ.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

2. 使用者點擊 ---> Bubble【換頁按鈕】中的按鈕【21-40】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(含簡介) --- 21-40】<br>
   系統回傳 ---> 文字【Netflix 新片(含簡介) 21-40】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/pD8CZX3.jpg" alt="文字【Netflix 新片(含簡介) 21-40】" width="278" height="476">
<img src="https://i.imgur.com/vuiLHSZ.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

3. 使用者點擊 ---> Bubble【換頁按鈕】中的按鈕【41-60】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(含簡介) --- 41-60】<br>
   系統回傳 ---> 文字【Netflix 新片(含簡介) 41-60】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/8WFgISD.jpg" alt="文字【Netflix 新片(含簡介) 41-60】" width="278" height="469">
<img src="https://i.imgur.com/ACLnE9U.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

## 新片(不含影片簡介)

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">

2. 使用者點擊 ---> Bubble【查詢】中的按鈕【新片】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片】<br>
   系統回傳 ---> Bubble【新片清單】
<img src="https://i.imgur.com/TVEf4Rn.jpg" alt="Bubble【新片清單】" width="278" height="335">

3. 使用者點擊 ---> Bubble【新片清單】中的按鈕【不含影片簡介】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(不含簡介) --- 1-20】<br>
   系統回傳 ---> 文字【Netflix 新片(不含簡介) 1-20】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/HZzH6uJ.jpg" alt="文字【Netflix 新片(不含簡介) 1-20】" width="278" height="476">
<img src="https://i.imgur.com/A5Gj3xp.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

4. 使用者點擊 ---> Bubble【換頁按鈕】中的按鈕【21-40】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(不含簡介) --- 21-40】<br>
   系統回傳 ---> 文字【Netflix 新片(不含簡介) 21-40】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/fv36iCT.jpg" alt="文字【Netflix 新片(不含簡介) 21-40】" width="278" height="476">
<img src="https://i.imgur.com/2DHfP1G.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

5. 使用者點擊 ---> Bubble【換頁按鈕】中的按鈕【41-60】<br>
   系統帶入 ---> 文字【Netflix 台灣 新片(不含簡介) --- 41-60】<br>
   系統回傳 ---> 文字【Netflix 新片(不含簡介) 41-60】 + Bubble【換頁按鈕】
<img src="https://i.imgur.com/C9w1KPv.jpg" alt="文字【Netflix 新片(不含簡介) 41-60】" width="278" height="476">
<img src="https://i.imgur.com/x9pQWEe.jpg" alt="Bubble【換頁按鈕】" width="278" height="476">

## Top 10 電影(含簡介)

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">

2. 使用者點擊 ---> Bubble【查詢】中的按鈕【Top 10 電影】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 電影】<br>
   系統回傳 ---> Bubble【Top 10 電影】
<img src="https://i.imgur.com/DJZK4ux.jpg" alt="Bubble【Top 10 電影】" width="278" height="337">

3. 使用者點擊 ---> Bubble【Top 10 電影】中的按鈕【含影片簡介】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 電影(含簡介)】<br>
   系統回傳 ---> 文字【Netflix 台灣 Top 10 電影(含簡介)】
<img src="https://i.imgur.com/ZntwLdE.jpg" alt="文字【Netflix 台灣 Top 10 電影(含簡介)】" width="278" height="476">

## Top 10 電影(不含簡介)

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">

2. 使用者點擊 ---> Bubble【查詢】中的按鈕【Top 10 電影】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 電影】<br>
   系統回傳 ---> Bubble【Top 10 電影】
<img src="https://i.imgur.com/DJZK4ux.jpg" alt="Bubble【Top 10 電影】" width="278" height="337">

3. 使用者點擊 ---> Bubble【Top 10 電影】中的按鈕【不含影片簡介】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 電影(不含簡介)】<br>
   系統回傳 ---> 文字【Netflix 台灣 Top 10 電影(不含簡介)】
<img src="https://i.imgur.com/tu8EZaZ.jpg" alt="文字【Netflix 台灣 Top 10 電影(不含簡介)】" width="278" height="476">

## Top 10 節目(含簡介)

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">

2. 使用者點擊 ---> Bubble【查詢】中的按鈕【Top 10 節目】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 節目】<br>
   系統回傳 ---> Bubble【Top 10 節目】
<img src="https://i.imgur.com/n281gKC.jpg" alt="Bubble【Top 10 節目】" width="278" height="334">

3. 使用者點擊 ---> Bubble【Top 10 節目】中的按鈕【含影片簡介】<br>
   自動帶入 ---> 文字【Netflix 台灣 Top 10 節目(含簡介)】<br>
   系統回傳 ---> 文字【Netflix 台灣 Top 10 節目(含簡介)】
<img src="https://i.imgur.com/qkMl0Iu.jpg" alt="文字【Netflix 台灣 Top 10 節目(含簡介)】" width="278" height="476">

## Top 10 節目(不含簡介)

1. 使用者輸入 ---> 文字【查詢】<br>
   系統回傳 ---> Bubble【查詢】
<img src="https://i.imgur.com/cfiKwoN.jpg" alt="Bubble【查詢】" width="278" height="369">

2. 使用者點擊 ---> Bubble【查詢】中的按鈕【Top 10 節目】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 節目】<br>
   系統回傳 ---> Bubble【Top 10 節目】
<img src="https://i.imgur.com/n281gKC.jpg" alt="Bubble【Top 10 節目】" width="278" height="334">

3. 使用者點擊 ---> Bubble【Top 10 節目】中的按鈕【不含影片簡介】<br>
   系統帶入 ---> 文字【Netflix 台灣 Top 10 節目(不含簡介)】<br>
   系統回傳 ---> 文字【Netflix 台灣 Top 10 節目(不含簡介)】
<img src="https://i.imgur.com/q2tupwU.jpg" alt="文字【Netflix 台灣 Top 10 節目(不含簡介)】" width="278" height="476">

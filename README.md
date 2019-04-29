# HomeWork-3

## Data processing
首先，由網站上所下載下來的數據如下圖。(file: stockmarket.csv)

![](https://imgur.com/P6M6d5A.png)

Date:日期(日/月/年)

Open Price:開盤價格

Close Price:收盤價格

High Price:盤中最高價

Low Price:盤中最低價

Volume:成交量

由上圖可以發現Date的格式無法直接拿來運用，因此藉由下面的程式碼將原本日期轉換成datetime可以處理的格式，並順便將資料標記成train和test兩個部分(依據作業要求)。

![](https://imgur.com/wt41r3h.png)
![](https://imgur.com/1EMxdM9.png)

時間的部分則又在經過下面程式碼，換算成timestamp，以利後續處理。

![](https://imgur.com/q0VgPMG.png)

理論上，下載下來的資料中，會被拿來預測的部分是Close Price(收盤價格)，但是由收盤價格並不能直接看出漲跌。因此，使用了下面的程式碼，計算出常見的漲跌幅(%)

![](https://imgur.com/3Fmletz.png)

之後，再將現有的表格以下列程式碼做選擇。

![](https://imgur.com/YtZeW2h.png)

最後，拿來進行初步運用的數據如下圖。並以上圖的Diff(漲跌幅)作為被預測的項目。

Open Price:開盤價格

High Price:盤中最高價

Low Price:盤中最低價

Volume:成交量

Time:時間，以timestamp表示

![](https://imgur.com/z93GcSZ.png)

## Regression

### Linear regression

將前面處理好的資料使用linear regression做training所得的結果如下。

![](https://imgur.com/nO8h6zt.png)

Testing的結果如下。

![](https://imgur.com/uIO1OUy.png)

將training所用到的data分別與Diff(漲跌幅)做散佈圖的結果如下，可以發現這幾個用來training的data與Diff(漲跌幅)似乎沒有太大的關連性。

![](https://imgur.com/xymrpWz.png)

### Ridge regression

由下圖可知，最佳 𝛼=0。

![](https://imgur.com/Vpxk7ws.png)

接著進行ridge regression所得的結果如下。可以看出與原本的linear regression並無太大差異

![](https://imgur.com/woFcwT5.png)

而由coefficients推測，Open price、High price及Low Price似乎與漲跌幅(Diff)關聯性稍微好一點

![](https://imgur.com/uSpEDAJ.png)

### Feature selection + Ridge regression

由前面的推測，選定Open price、High price及Low Price來進行Linear regression及Ridge regression。結果如下：

![](https://imgur.com/AQoakEj.png)

Ridge regression結果則如下：

![](https://imgur.com/jgguBMf.png)

綜合前述的結果，可以看出在經過Feature selection後的Linear regression及Ridge regression的MSE，都比沒有經過Feature selection的MSE差一些。


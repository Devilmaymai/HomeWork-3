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

### #Linear regression

將前面處理好的資料使用linear regression做training所得的結果如下。

![](https://imgur.com/nO8h6zt.png)

Testing的結果如下。

![](https://imgur.com/uIO1OUy.png)

將training所用到的data分別與Diff(漲跌幅)做散佈圖的結果如下，可以發現這幾個用來training的data與Diff(漲跌幅)似乎沒有太大的關連性。

![](https://imgur.com/xymrpWz.png)

### #Ridge regression

由下圖可知，最佳 𝛼=0。

![](https://imgur.com/Vpxk7ws.png)

接著進行ridge regression所得的結果如下。可以看出與原本的linear regression並無太大差異

![](https://imgur.com/woFcwT5.png)

而由coefficients推測，Open price、High price及Low Price似乎與漲跌幅(Diff)關聯性稍微好一點

![](https://imgur.com/uSpEDAJ.png)

### #Feature selection + Ridge regression

由前面的推測，選定Open price、High price及Low Price來進行Linear regression及Ridge regression。

Linear regression結果如下：

![](https://imgur.com/AQoakEj.png)

Ridge regression結果則如下：

![](https://imgur.com/jgguBMf.png)

### #Discussion — regression

綜合前述的結果，可以看出在經過Feature selection後的Linear regression及Ridge regression的MSE，都比沒有經過Feature selection的MSE差一些。
而這其實由Scatter plot就可以看出端倪了。

此外，由Time-Diff的Scatter plot中可以看出漲跌幅與時間似乎有小循環。
![](https://imgur.com/uoQbWTB.png)

因此，將時間轉換成Year、Month、date及Q(季)之後再進行進行一次前面的程式運算。(file: HW3_Linear_Regression_Trail2)

轉換程式碼：
![](https://imgur.com/VXnX8zA.png)

#### ->Linear regression

training的結果較前面的好，但是套用到testing set時結果就比前面的結果差。
![](https://imgur.com/a2Xcyx0.png)

#### ->Ridge regression

test結果還是較差
![](https://imgur.com/bn8Ert7.png)

#### ->Feature selection ("Open Price", "High Price", "Low Price", "Q")

test結果也是較差
![](https://imgur.com/f8yh94I.png)

#### ->Feature selection + ridge regression

test結果也還是較差
![](https://imgur.com/hY4WcH7.png)

#### ->Summary

到這邊，已經嘗試了我覺得可能可以改進的方法。但結果並不如預期，不過這些其實由scatter plot就可以看出端倪。
除非有更關鍵的資料或是額外的公式引入，不然應該是無法改進這個結果了。


## SVM
資料處理沿用前面的部分。處理完的資料則選用"Month"與"Open Price"對"Diff"做訓練。原因主要有以下兩點：

1.根據下面的兩個scatter plot，可以看出前面討論的小循環可能是以年為單位，因此選用其下一級的單位"Month"來做training。
![](https://imgur.com/eLIItPG.png)
![](https://imgur.com/dIzKicn.png)

2."High Price"(當日最高)、"Low Price"(當日最低)及"Volume"(成交量)，基本上為當日收盤結算時才能確定的資料，因此拿來預測漲跌(Diff)不太合理。
所以這裡僅選擇開盤就能得知的"Open Price"來當作training用的第二種data。

### #SVM - linear

使用linear的kernal，並且令Penalty = 0.05。所做出來的結果如下圖。可以看到預測的準確度僅有50.4%，基本上跟擲硬幣沒兩樣。
![](https://imgur.com/4kkNrqQ.png)

### #SVM - sigmoid

使用sigmoid的kernal，並且令Penalty = 0.05，做出來的結果如下圖。預測的準確度僅有49.6%，也是屬於擲硬幣等級的準確度。
![](https://imgur.com/f9SeKQS.png)

由於兩者的程式碼僅差在kernal = linear或kernal = sigmoid，因此僅附上一個檔案。

### #Discussion - SVM

無論是linear或是sigmoid，在training完後test的準確度都不理想，很有可能是拿來training的數據與漲跌幅(Diff)並無太大關聯導致。
但在嘗試過其他的trainig set之後，得到的預測準確度都差不多，沒有值得放上來的結果。
所以，推論應該是所下載下來的數據與漲跌幅(Diff)本身就沒有直接的關聯。

## Neural network

資料處理沿用前面的部分，不果這次會有兩種時間類型的資料，一個是時間為timestamp格式("Time")，另一個則是年月日季("Year","Month","Date","Q")的格式。

兩種時間類型的資料則分別使用relu及sigmoid的hidden layer activation function各training及預測一次。

### #Neural network - timestamp

檔案：HW3_Neural_Network

![](https://imgur.com/i7NCmfz.png)
![](https://imgur.com/72wZSeq.png)

結果如下：

#### ->relu
![](https://imgur.com/L1X3G7D.png)

#### ->sigmoid
![](https://imgur.com/DesYjCp.png)


### #Neural network - Year, month, date, Q

檔案：HW3_Neural_Network_Trail2

參數的setup基本上跟前面一樣

結果如下：

#### ->relu

![](https://imgur.com/UdjGZuV.png)

#### ->sigmoid

![](https://imgur.com/8Qm5lnB.png)

### #Discussion - Neural network

由前面的四個結果，可以看到預測準確度幾乎都在50%附近。兩種時間資料並沒有造成太大的差別。
很油可能漲跌幅並無罰有這樣的資料預測出來

## 總結

比較了三個方式，其實並沒有看出哪種calssifiers能夠提供較好的預測。
這很大概就是garbage in, grabage out的概念，使用了不太有關聯的數據，就無法得到有意義結果。
不過也可能是因為我對於資料分析的資歷尚淺，所以無法有效利用手邊的數據與工具所導致。


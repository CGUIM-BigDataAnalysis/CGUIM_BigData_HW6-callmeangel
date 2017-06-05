台灣旅遊與僑生來台就學
================

組員姓名
--------

資管二甲 B0344122 楊守仁

分析議題背景
------------

從網路上的open data收集了歷年來台旅客的國藉及現時台灣各大專院校外藉學生等資料從而了解台灣的風光和旅遊，有沒有帶起外國人對台灣的興趣並且來台就/升學。

分析動機
--------

作為外國長大的僑生的一分子，藉著這個機會了解「同是天涯淪落人」的留台外國人的在台灣甚至在長庚的狀況。

使用資料
--------

File\_1940是各級學校僑生資料 visitorstatictis1是自1956年以來來台旅客人數及國藉資料 Student\_RPT\_05是現時台灣各大專院校外藉學生資料包括人數國藉科系學制等資料

\*還有一份是"大專校院僑生及畢業生人數—按性別、校別與僑居地別分"資料，但資料尚有一些問題，所以尚未載入，但是連同作業一同commit

載入使用資料們

``` r
#這是R Code Chunk
library(readr)
File_1940 <- read_csv("~/Desktop/bigdata/File_1940.csv",skip = 3)
```

    ## Warning: Missing column names filled in: 'X1' [1], 'X2' [2]

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_character(),
    ##   X2 = col_character(),
    ##   計 = col_character(),
    ##   `公私立
    ## 大　學` = col_character(),
    ##   `國防
    ## 醫學院` = col_character(),
    ##   `僑　生
    ## 先修部` = col_character(),
    ##   `專科
    ## 學校` = col_character(),
    ##   高國中 = col_character(),
    ##   `職業
    ## 學校` = col_character(),
    ##   國小 = col_integer(),
    ##   `各級
    ## 補校` = col_character(),
    ##   海青班 = col_integer()
    ## )

    ## Warning: 1 parsing failure.
    ## row col   expected    actual                              file
    ##   1  -- 12 columns 9 columns '~/Desktop/bigdata/File_1940.csv'

``` r
visitorstatictis1 <- read_csv("~/Desktop/bigdata/visitorstatictis1.csv")
```

    ## Warning: Missing column names filled in: 'X2' [2], 'X3' [3], 'X4' [4],
    ## 'X5' [5], 'X6' [6], 'X7' [7], 'X8' [8], 'X9' [9], 'X10' [10]

    ## Parsed with column specification:
    ## cols(
    ##   `Visitor  Arrivals, 1956-2015` = col_character(),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character(),
    ##   X7 = col_character(),
    ##   X8 = col_character(),
    ##   X9 = col_character(),
    ##   X10 = col_character()
    ## )

``` r
Student_RPT_05 <- read_csv("~/Desktop/bigdata/Student_RPT_05.csv")
```

    ## Warning: Missing column names filled in: 'X1' [1], 'X3' [3], 'X4' [4],
    ## 'X5' [5], 'X6' [6], 'X7' [7], 'X8' [8], 'X9' [9]

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_character(),
    ##   `05.僑生、港澳生數` = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character(),
    ##   X7 = col_character(),
    ##   X8 = col_character(),
    ##   X9 = col_character(),
    ##   `僑生總計：` = col_character(),
    ##   `43833` = col_character(),
    ##   `僑生男：` = col_character(),
    ##   `21806` = col_character(),
    ##   `僑生女：` = col_character(),
    ##   `22027` = col_character()
    ## )

資料處理與清洗
--------------

``` r
#資料清洗
names(File_1940)[1] <-"學年度"
names(File_1940)[2] <-"總計"
names(File_1940)[3] <-"計"
names(File_1940)[4] <-"公私立大學"
File_1940 <- File_1940[-1,]

visitorstatictis1 <- visitorstatictis1[-1,]
visitorstatictis1 <- visitorstatictis1[-1,]
names(visitorstatictis1)[1] <-"year"
names(visitorstatictis1)[2] <-"總計人數"
names(visitorstatictis1)[3] <-"成長率"
names(visitorstatictis1)[4] <-"指數"
names(visitorstatictis1)[5] <-"外籍旅客人數"
names(visitorstatictis1)[6] <-"外籍旅客成長率"
names(visitorstatictis1)[7] <-"外籍旅客指數"
names(visitorstatictis1)[8] <-"華僑旅客人數"
names(visitorstatictis1)[9] <-"華僑旅客成長率"
names(visitorstatictis1)[10] <-"華僑旅客指數"

Student_RPT_05$X1 <- NULL
Student_RPT_05 <- Student_RPT_05[-1,]
Student_RPT_05 <- Student_RPT_05[-1,]
Student_RPT_05 <- Student_RPT_05[-1,]
names(Student_RPT_05)[1] <-"學年度"
names(Student_RPT_05)[2] <-"設立別"
names(Student_RPT_05)[3] <-"學校類別"
names(Student_RPT_05)[4] <-"學校代碼"
names(Student_RPT_05)[5] <-"學校名稱"
names(Student_RPT_05)[6] <-"系所代碼"
names(Student_RPT_05)[7] <-"系所名稱"
names(Student_RPT_05)[8] <-"學制"
names(Student_RPT_05)[9] <-"僑生總計"
names(Student_RPT_05)[10] <-"僑生男"
names(Student_RPT_05)[11] <-"僑生女"
names(Student_RPT_05)[12] <-"港澳生總計"
names(Student_RPT_05)[13] <-"港澳生男"
names(Student_RPT_05)[14] <-"港澳生女"



#各級學校僑生現況
presentofoversea<-head(File_1940[order(File_1940$"學年度",decreasing = T),],10)

#國立大學僑生現況
presentofnational<-Student_RPT_05[grepl("公立",Student_RPT_05$"設立別"),]

#私立大學僑生現況
presentofprivate<-Student_RPT_05[grepl("私立",Student_RPT_05$"設立別"),]

#長庚大學僑生現況
test105<-Student_RPT_05[grepl("105",Student_RPT_05$"學年度"),]
testcgu<-test105[grepl("長庚大學",test105$"學校名稱"),]

knitr::kable(
  head(presentofoversea)
)
```

學年度 總計 計 公私立大學 國防 醫學院 僑　生 先修部 專科 學校 高國中 職業 學校 國小 各級 補校 海青班 ------- ------ ------ ----------- ------------ -------------- ---------- ------- ---------- ----- ---------- ------- 99 15525 14664 11951 75 1554 109 545 147 283 - 861 98 14788 14109 11228 72 1628 152 602 160 267 - 679 97 13223 12661 10047 74 1344 186 605 220 132 53 562 96 12556 12137 9450 75 1348 226 576 263 123 76 419 104 25944 24765 21802 88 990 58 642 779 400 6 1179 103 22685 21437 18741 81 1248 64 918 0 385 NA 1248

``` r
knitr::kable(
  head(presentofnational)
)
```

| 學年度 | 設立別 | 學校類別 | 學校代碼 | 學校名稱     | 系所代碼 | 系所名稱               | 學制         | 僑生總計 | 僑生男 | 僑生女 | 港澳生總計 | 港澳生男 | 港澳生女 |
|:-------|:-------|:---------|:---------|:-------------|:---------|:-----------------------|:-------------|:---------|:-------|:-------|:-----------|:---------|:---------|
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 140101   | 教育學系               | 學士班(日間) | 11       | 2      | 9      | 2          | 0        | 2        |
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 140101   | 教育學系               | 碩士班(日間) | 1        | 0      | 1      | 1          | 0        | 1        |
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 140101   | 教育學系               | 博士班       | 1        | 0      | 1      | 0          | 0        | 0        |
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 140203   | 華語文教學碩士學位學程 | 碩士班(日間) | 6        | 1      | 5      | 0          | 0        | 0        |
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 140401   | 幼兒教育研究所         | 碩士班(日間) | 0        | 0      | 0      | 2          | 0        | 2        |
| 101    | 公立   | 一般大學 | 0001     | 國立政治大學 | 220201   | 中國文學系             | 學士班(日間) | 8        | 2      | 6      | 11         | 5        | 6        |

``` r
knitr::kable(
  head(presentofprivate)
)
```

| 學年度 | 設立別 | 學校類別 | 學校代碼 | 學校名稱 | 系所代碼 | 系所名稱     | 學制         | 僑生總計 | 僑生男 | 僑生女 | 港澳生總計 | 港澳生男 | 港澳生女 |
|:-------|:-------|:---------|:---------|:---------|:---------|:-------------|:-------------|:---------|:-------|:-------|:-----------|:---------|:---------|
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 210101   | 美術學系     | 學士班(日間) | 1        | 0      | 1      | 3          | 2        | 1        |
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 210101   | 美術學系     | 碩士班(日間) | 1        | 1      | 0      | 0          | 0        | 0        |
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 220201   | 中國文學系   | 學士班(日間) | 3        | 1      | 2      | 5          | 2        | 3        |
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 220201   | 中國文學系   | 碩士班(日間) | 0        | 0      | 0      | 1          | 0        | 1        |
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 220301   | 外國語文學系 | 學士班(日間) | 15       | 6      | 9      | 10         | 1        | 9        |
| 101    | 私立   | 一般大學 | 1001     | 東海大學 | 220301   | 外國語文學系 | 碩士班(日間) | 1        | 1      | 0      | 0          | 0        | 0        |

``` r
knitr::kable(
  head(testcgu[order(testcgu$"學制",decreasing = T),],)
)
```

| 學年度 | 設立別 | 學校類別 | 學校代碼 | 學校名稱 | 系所代碼 | 系所名稱     | 學制         | 僑生總計 | 僑生男 | 僑生女 | 港澳生總計 | 港澳生男 | 港澳生女 |
|:-------|:-------|:---------|:---------|:---------|:---------|:-------------|:-------------|:---------|:-------|:-------|:-----------|:---------|:---------|
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 230303   | 工業設計學系 | 學士班(日間) | 0        | 0      | 0      | 1          | 0        | 1        |
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 340302   | 工商管理學系 | 學士班(日間) | 2        | 1      | 1      | 3          | 3        | 0        |
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 340901   | 醫務管理學系 | 學士班(日間) | 2        | 1      | 1      | 6          | 1        | 5        |
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 420503   | 生物醫學系   | 學士班(日間) | 8        | 3      | 5      | 5          | 2        | 3        |
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 480109   | 資訊管理學系 | 學士班(日間) | 0        | 0      | 0      | 2          | 1        | 1        |
| 105    | 私立   | 一般大學 | 1009     | 長庚大學 | 520103   | 電子工程學系 | 學士班(日間) | 0        | 0      | 0      | 3          | 3        | 0        |

探索式資料分析
--------------

圖文並茂圖文並茂

``` r
#這是R Code Chunk
```

期末專題分析規劃
----------------

期末專題會結合手上的資料，按照不同國藉的僑生人數製作圖表及一個世界分佈地圖。然後還會了解數據，找出一些關於僑生的有趣現象在課堂報告跟老師和同學分享。

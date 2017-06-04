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
File_1940 <- read_csv("~/Desktop/bigdata/File_1940.csv")
```

    ## Warning: Missing column names filled in: 'X2' [2], 'X3' [3], 'X4' [4],
    ## 'X5' [5], 'X6' [6], 'X7' [7], 'X8' [8], 'X9' [9], 'X10' [10], 'X11' [11],
    ## 'X12' [12]

    ## Parsed with column specification:
    ## cols(
    ##   各級學校及海青班在學僑生人數 = col_character(),
    ##   X2 = col_character(),
    ##   X3 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character(),
    ##   X7 = col_character(),
    ##   X8 = col_character(),
    ##   X9 = col_character(),
    ##   X10 = col_character(),
    ##   X11 = col_character(),
    ##   X12 = col_character()
    ## )

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
Student_RPT_05 <- read_csv("~/Desktop/bigdata/Student_RPT_05.csv",skip = 2)
```

    ## Warning: Missing column names filled in: 'X1' [1], 'X4' [4], 'X5' [5],
    ## 'X6' [6], 'X11' [11], 'X12' [12], 'X14' [14], 'X15' [15]

    ## Parsed with column specification:
    ## cols(
    ##   X1 = col_character(),
    ##   學年度 = col_integer(),
    ##   學校 = col_character(),
    ##   X4 = col_character(),
    ##   X5 = col_character(),
    ##   X6 = col_character(),
    ##   系所代碼 = col_integer(),
    ##   系所名稱 = col_character(),
    ##   學制 = col_character(),
    ##   僑生 = col_character(),
    ##   X11 = col_character(),
    ##   X12 = col_character(),
    ##   港澳生 = col_character(),
    ##   X14 = col_character(),
    ##   X15 = col_character()
    ## )

    ## Warning: 8 parsing failures.
    ##   row      col               expected                                                                                                                                                     actual                                   file
    ##  8241 系所代碼 no trailing characters A1                                                                                                                                                         '~/Desktop/bigdata/Student_RPT_05.csv'
    ## 11124 系所代碼 no trailing characters A1                                                                                                                                                         '~/Desktop/bigdata/Student_RPT_05.csv'
    ## 12569 系所代碼 no trailing characters B7                                                                                                                                                         '~/Desktop/bigdata/Student_RPT_05.csv'
    ## 13388 學年度   an integer             統計說明：                                                                                                                                                 '~/Desktop/bigdata/Student_RPT_05.csv'
    ## 13389 學年度   no trailing characters . 本表設立別係指【公立、私立】大學校院等分類；學校類別係指【一般大學、技專校院】等；學校名稱係指【學校校名】；系所名稱係指【學校系、所、學位學程之名稱】。 '~/Desktop/bigdata/Student_RPT_05.csv'
    ## ..... ........ ...................... .......................................................................................................................................................... ......................................
    ## See problems(...) for more details.

資料處理與清洗
--------------

各級學校僑生現況
================

presentofoversea&lt;-head(File\_1940\[order(File\_1940$"各級學校及海青班在學僑生人數",decreasing = T),\],10) View(presentofoversea)

國立大學僑生現況
================

presentofnational&lt;-Student\_RPT\_05\[grepl("公立",Student\_RPT\_05$"學校"),\] View(presentofnational)

私立大學僑生現況
================

presentofprivate&lt;-Student\_RPT\_05\[grepl("私立",Student\_RPT\_05$"學校"),\] View(presentofprivate)

長庚大學僑生現況
================

test105&lt;-Student\_RPT\_05\[grepl("105",Student\_RPT\_05$"學年度"),\] testcgu&lt;-test105\[grepl("長庚大學",test105$X6),\]

knitr::kable( head(presentofoversea) )

knitr::kable( head(presentofnational) )

knitr::kable( head(presentofprivate) )

knitr::kable( head(testcgu\[order(testcgu$"學制",decreasing = T),\],) ) \`\`\`

探索式資料分析
--------------

圖文並茂圖文並茂

``` r
#這是R Code Chunk
```

期末專題分析規劃
----------------

期末專題會結合手上的資料，按照不同國藉的僑生人數製作圖表及一個世界分佈地圖。然後還會了解數據，找出一些關於僑生的有趣現象在課堂報告跟老師和同學分享。

算是第一次參加AIS3 這次解出8題 不含Welcome

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.001.png)

Crypto題型

1. SC

基本上是水題，給出enc和原code馬上就知道是將字替換掉

將flag的enc拉到最後一行可以很明顯的知道這就是flag

5xvJ{IVnCDwT\_I24t6W626DVw\_ODPzJi\_FDMz\_awVFw\_PWmDw6J86\_m66cOa}

對應到

AIS3{s0lving\_sub5t1tuti0n\_ciph3r\_wi7h\_kn0wn\_p14int3xt\_4ttack}

Misc題型

1. Excel

載下來一個xlsm，一開始的思路是裡面可能有藏東西，因此用binwalk來看，發現有很多資料在裡面

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.002.png)

在這裡面糾結了很久，最後發現也是水題，用Excel打開後就有提示說是否啟用巨集

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.003.png)

想到說可能直接執行巨集跑，逐行執行後發現flag

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.004.png)

1. Gift in the dream

載下來是個gif檔，一開始用Convert拆掉，看了一下大約50張圖組成，再用Stegsolve分析，但好像沒有隱藏圖片

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.005.png)

想說用strings看看，結果發現有提示

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.006.png)

提到了animation lagging，再想到有CTF是利用gif每張圖片的間隔來寫flag的

就用identify看看時間，果然看到65 73 83 51，直接就知道是ascii的AIS3

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.007.png)

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.008.png)


Reverse題型

1. Time Management

Reverse題型，先跑一次程式看看

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.009.png)

發現一直卡在這邊

接著丟到IDA Pro看看，第一個想法是讓程式直接跳到flag那邊

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.010.png)

跳過去的是126B，就將jle無腦改成jge試試看

發現程式直接結束了

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.011.png)

好吧要認真看了

發現Text有一個看起來像flag的東西

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.012.png)

結果送失敗QQ他顯示使用strings 但能看到這個通常這句話也沒什麼幫助QQ

接著看到20B0

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.013.png)

看到%c感覺起來他的flag在程式執行的時候 是有被一個一個字元輸出出來的

這邊也有提示說跟time有關

接著就找跟time有關的，發現0x 1230有call sleep function，而前面帶的參數是0x8763等於要跑9個小時才sleep完，看來這個就是答案了

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.014.png)

將秒數改成1

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.015.png)

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.016.png)

Web題型

1. Simple File Uploader

基本上進去就一個上傳檔案的Service

看了一下source code發現副檔名擋一堆

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.017.png)

然後檔案內容也是擋一堆

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.018.png)

副檔名的話 繞過很簡單，因為已經限定字串了，因此用phP就能繞過

檔案內容想了很久，最後想到能用<?php $\_GET['a']($\_GET['b']);?>來繞過

就upload上去，發現upload成功，沒有被擋下來

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.019.png)

接著到上傳的路徑

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.020.png)

感覺應該成功了

接著就是?a=system&b=ls /

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.021.png)

whoami

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.022.png)

可以開始找flag了

有一個das\_ist\_die\_fL4g.txt發現cat不到ls -la才發現是權限不夠

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.023.png)

就先把目光轉到rUn\_M3\_t0\_9et\_fL4g再說，畢竟跑他的權限是夠的

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.024.png)

?a=system&b=../../../../../rUn\_M3\_t0\_9et\_fL4g

結束

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.025.png)

1. Poking Bear

一開始的想法就是先看F12能不能點SECRET BEAR

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.026.png)

但看起來就算開啟也點不進去

接著就把全部的Go poke him ! 點開就能發現第一個分頁是/bear/5 第二個是29 接著是82、327、350、777、999

不能點擊的SECRET BEAR卡在350跟777中間

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.027.png)

找到是499不一樣

對499 Poke! 一下，發現跳出我不是bear poker

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.028.png)

看到這個基本上就知道是吃Cookie

把Cookie當中的human參數改成bear poker後拿到flag

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.029.png)

Pwn題型

1. SAAS - Crash

載下來後發現並沒有任何call bin/sh之類的function，既然是40分的題目應該是水題，直接連上去發現可以新增跟查看，總之塞爆就對了，不斷的新增和查看最後就會吐出flag了

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.030.png)

1. BOF2WIN

看題目就知道overflow到某個地方就好

先看checksec

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.031.png)

打開載下來的bof2win.c檔可以知道要跳到get\_the\_flag()這個function，所以有NX保護也沒差

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.032.png)

可以看到gets的buffer只有16

其實這邊根據以往的經驗其實可以大概猜得出來了，但還是gdb一下，因為沒裝peda所以先用pwntools的cyclic

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.033.png)

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.034.png)

Segmentation fault後看暫存器

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.035.png)

從cyclic可以知道到壓到rbp是20因此壓完rbp是24

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.036.png)

接著看get\_the\_flag的address

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.037.png)

所以payload就有了

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.038.png)

送出去 get flag ~

![](Aspose.Words.9b38d774-4e64-4a6f-9dc5-97260abb79ed.039.png)

# compi test
> 供大家編輯 
> 知道答案的就寫
> 不知道的就看
> 有衝突的\n寫你覺得對的然後comment討論
> 不能編要講
> 阿盡量不要讓老師知道

## 1. 是非(107) 30%

1. Right-most derivation 與 bottom-up 剖析的過程一模一樣。 X
> 長出來一樣，但方向相反[name=筱雲]
> A bottom-up parser traces a rightmost derivation in reverse. from 6.1 P4[name=SwiMin]
2. 一個 sentential form 的 handle 為此 sentential form 中最左邊的 simple phrase。 O
3. LL(k)與 LR(k)中的 k 是一樣的意思，都是看下 k 個 input 符號來決定下一步的動作。 O
4. Recursive-decent parser 屬於 top-down 的一種剖析方式。 O
6. 同一個 non-terminal 為左手邊(LHS)的不同規則其 Predict set 都相同。 X，不一定(why)

    若predict(A)的First(A)為landa,其predict為Follow
8. 存在某個 CFG 無法使用 LR(k)來剖析。 O
9. 一個 sentential form 的 handle 為在 LR(k)剖析時要被 reduced 的部分。 O
10. Shift-reduce parser 中的 shift 表示將 input 的符號放入堆疊。 O
11. 所有的 CFGs 都可以使用 LR(k)來剖析。 X，不是所有(why)
> ambiguous 語法就不行了 [name=柏安]
> 參考 : [Stack Overflow](https://stackoverflow.com/questions/25343014/can-an-lr1-parser-parse-this-grammar)
12. 要判斷一個文法是否為 LL(1)可以利用文法規則(production)的 Predictive set 來判斷。 O
13. 一個文法不為 LL(1)的原因主要分為兩大類，即共同前置因子(common prefixes)與右遞迴。 X，左遞迴
14. 如果一個 if 就有一個 endif 對應的話，就不會有 dangling-else 的問題。 O
15. 一個 ambiguous 的文法可以用 LR(k)剖析 (即 LR(k) deterministically 地剖析)。 X，不行(why)
> ambiguous 的語法在 parser 剖析樹就有問題了，再強的 parser 方法都不行 [name=柏安]
16. LR(0)的 0 表示在剖析時(parsing)，不用看 input 的符號。 X，要看(看啥)
> LR(0) 不是代表剖析的時候參考符號的數量
> 而是代表**建構剖析表的時候直接往下生成而不用看下一個符號** (我筆記當下做是這樣，求證) [name=柏安]
> 解釋正確 [name=靜]
> the "0" in LR(0) refers not to the lookhead at parse-time, but refer to the lookhead used in constructing the parse table
17. LR(k)又稱 shift-reduce，其中 reduce 表將堆疊內的 non-terminal 換成某條文法規則的右手邊(RHS)。 X
> non-terminal 換成某條文法規則的左手邊。 [name=筱雲]
18. 一個 LR(k)的剖析表格，其行數(column number)至少為 kn，其中 n 為不同 token 的數量。 X，n為一個token的size
20. 在 LR(k)的剖析表中，如果有狀態為 inadequate state 表示某些格子(cell or entry)存在衝突。 O
21. LR(k)的衝突可以分成 shift/shift 衝突與 shift/reduce 衝突。 X，沒有Shift/Shift ，有Reduce/Reduce
22. 當一個 ambiguous 的文法使用 shift-reduce 剖析發生衝突時可以用看更多的 lookahead 符號，或是使用更強大(powerful)的剖析方法來解決。 O
22. 對同一個文法而言，使用 LALR(k)的狀態數量與使用 LR(0)的狀態數量是相同的。 O
23. 在建構 LALR(1)的 CFSM 時，如果有兩個狀態 shift 一個符號後的 item (或 items) 相同則狀態會共用且使用聯集形成其 ItemFollow()集合。O
24. SLR(1)可以剖析的文法範圍比 LALR(1)還要多。 X,less than
> ![](https://i.imgur.com/mMfSCaI.png)[name=SwiMin]
25. 對同一個文法而言，使用 LALR(k)的狀態數量與使用 LR(0)的狀態數量是相同的。 O
26. LR(1)的狀態要共用時，Item 內容要相同，但 ItemFollow 的內容可以不同。 X,反了(詳細)
>ItemFollow 內容不同就要發生 Reduce/Reduce 衝突了 (?) [name=柏安]
27. LR(1)狀態數量與 LALR(1)相同。 X，是LR(0)(差別?)
> 只知道 LALR 根據 CFSM 推斷，而 CFSM 由 LR(0) 方式構建而成 [name=柏安]


## 2. parser table 的壓縮
**有過程更好(會的幫補XD)**
> 根據老師上課筆記 [name=雲]
    1. Vtable 每個 row 內的所有非空白的 entry 存入表格中，保持原來表格的相對位置
    2. Rtable 每個 row 內所有非空的entry 存入表格時，記錄其與原表格的位移位置
    
![](https://i.imgur.com/VZqACAz.png)
![](https://i.imgur.com/1W9Lym7.png)

**Double-offset indexing compression**
1. R 表格決定了 **當遇到哪個 Row 的時候需要在 V 表格主動位移多少格**
2. 所以在 R 表格中 Row 1 完全不需要位移 (因為是最早被放進去的)，L 與 P 分別放入 V 表格的 index 1 與 index 4 （注意 : 決定 V 表格要放入哪個 Index 是要看最前面表格的 **column** )
3. 接下看 Row 2 ，把 Q 與 R 放入 V 表格的 index 2 與 index 5 當中
4. Row 3 以此類推
5. Row 4 的時候，**放入 W 到 V 表格時與 L 發生衝突**
    - 所以只能往下放
    - 但是又跟 Q 發生衝突，只好又往下放
    - 於是衝突到最後發現 V 表格中的 Index 6 沒有放東西，就把 W 放進去
    - X 接著往下放，於是在**R表格**當中 Row 4 的 Shift 為 5 (因為放 W 的時候就是往下移動 5 格，而 X 是接著放的，如果該 Row 後面有其他需要放的也是根據此位移來放)
6. Row 6 以此類推，於是你就會做了 :D


## 3. 證明 LR(k) 字串
![](https://i.imgur.com/TSPYJQT.png)

![](https://i.imgur.com/L7MeI7f.jpg)
![](https://i.imgur.com/r4zhuwK.jpg)

>換一下高清版本XD[name=予樺]
<!--[](https://i.imgur.com/pS6W50u.png)
![](https://i.imgur.com/GpAGXfu.png)-->



    https://en.wikipedia.org/wiki/LR_parser

## 4. 解釋一個文法不為SLR(k) 卻是 LALR(k) p.105
![](https://i.imgur.com/L6Srof4.png)
    SLR() 讓出現在folow內的使用reduce，但是shift與follow()有交集，則無法判斷應該shift或是 reduce
    解決方式，使用 LALR(k)
    
    https://en.wikipedia.org/wiki/Simple_LR_parser
    https://en.wikipedia.org/wiki/LALR_parser

> 另外一種解釋版本 (?)
> 衝突發生的地方在 State 3
> SLR(k) 的作法是 **前一個 nonterminnal 的 Follow() 不斷往下看**
> 所以在 State 3 發生 Shift/Reduce 衝突的時候，SLR(k) 
> 會檢查 Follow(A) = {b, c, $} => 與 State 3 的 S -> ac 集合衝突
> 故這文法非 SLR(k)，但是 LALR(k) **是看所有 Follow() 而不是單看前一個 nonterminal**
> [name=柏安]

![](https://i.imgur.com/QPh9CJ0.png)

## 5. 文法不為 LR(k) 
看k看不到的時候
predict為空字串?

此圖的文法使用 LR(k)無法解決，根據rightmost推導，必須知道最後的a 在第幾個位子，但文法推導後，因為可以一直代換 plus num 到無限大，所以即使看k個也無法看到最後的 a 
![](https://i.imgur.com/0ACOIbl.png)
![](https://i.imgur.com/PLba3G2.png)



## 6. 解釋文法不為LALR(k) 
  
item 一樣 ,follow 一樣才能合併
![](https://i.imgur.com/BJ4OTEq.png)
![](https://i.imgur.com/loxkmUB.png)
![](https://i.imgur.com/LX8iSnF.png)

    https://en.wikipedia.org/wiki/LALR_parser
    
    
## 7. 建立 LALR(k)的CSFM
![](https://i.imgur.com/TI4sbet.png)
![](https://i.imgur.com/jAxmtaH.png)
![](https://i.imgur.com/12XLJ7n.jpg)

    
## 8. 寫出 closure1() 的結果

![](https://i.imgur.com/Lv6TYao.png)
![](https://i.imgur.com/v3N5Gw0.png)
![](https://i.imgur.com/9onZtz6.png)
 
    

<!--大家晚安，早點睡-->
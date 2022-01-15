# 111111
## 22222
### 33333
#### 44444
##### 55555

段落結尾加兩個空格  
可以空行

或是直接用空行來換行

*斜體*  
**粗體**  
***斜粗體***

分隔線
***

~~刪除線~~

<u>下劃線</u>


註釋[^mark]  

[^mark]:test

無號
* 第一
* 第二
* 第三

有號
1. 第一
2. 第二
3. 第三

嵌套

1. 1
   * 1-1
   * 1-2
2. 2
   * 2-1
   * 2-2

> 這是一個引用

> 嵌套引用
>> 第二層
>>>第三層

>多個  
>可變  
>一個block 

* 在列表中
   > 引用   
   > ">"也要縮排
* testestest

code  
`printf()` 函數  
block code
```cpp
#include<iostream> 
using namespace std;

int main()
{
    cout << "hello, world!"<< endl;
    return 0;
}
```

[連結](https://www.google.com/)  
[連結也可以使用相對路徑](test.md)

連結也可用變數

[Google][1]

[youtube][video]

[1]: https://www.google.com/
[video]: https://www.youtube.com/

![圖片](https://storage.googleapis.com/www-cw-com-tw/article/201810/article-5bd182cf13ebb.jpg)

用 \<img>標籤可以調整圖片大小

<img src ="https://storage.googleapis.com/www-cw-com-tw/article/201810/article-5bd182cf13ebb.jpg" width="50%">

# 表格
| 左對齊testtest | testtesttest右對齊 | testtesttest居中 |
| :---  | ----: | :----: |
| 單元格 |  單元格 | 單元格 |
| 單元格 |  單元格  | 單元格 |

不在markdown 涵蓋的標籤 都可以用html來寫

<kbd>Ctrl</kbd>+<kbd>Alt</kbd>+<kbd>Del</kbd> 啟動工作管理員

\*\*反斜槓轉義\*\*

另外可以用LaTeX來顯示公式

[Latex](Latex.md)


  
[:blush:emoji](https://gist.github.com/rxaviers/7360908)
<!--    註解    -->

----
主要參考[菜鳥教程](https://www.runoob.com/markdown/md-advance.html)並簡化為方便自己理解的筆記
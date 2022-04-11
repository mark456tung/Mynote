安裝好後須先初始化  
```git config --global user.name "username"  ```  
```git config --global user.email user@email ```

![工作流程](image/git-process.png)
圖來自[菜鳥教程](https://www.runoob.com/git/git-workflow.html) 
查看檔案是否有追蹤  
```git status -s  ```

列出本地分支  
```git branch  ```

創建分支  
```git branch $branchname```

切換分支  
```git checkout $branchname  ```

創建分支並切換  
```git branch -b $branchname ```

刪除分支  
```git branch -d $branchname ```

合併A分支到當前分支  
```git merge --no-ff A  ```
--no-ff使用no-fast-farward merge git默認使用fast-farward merge ， 但no-fast-farward merge 可以保留更多版本訊息如圖所示  
![fast-farward](image/fast-farward%20merge.png)
![no-fast-farward](image/no-fast-farward%20merge.png) 
若有衝突需手動修改，然後再git add

查看提交歷史  
```git log ```    

查看提交歷史(精簡版)  
```git log --oneline```

```git log --decorate ```

找某人提交的版本  
```git log --author=mark --oneline -5```


## 待整理
僅挑選要的版本合併進來cherry-pick




## git參考
[為你自己學 Git](https://gitbook.tw/)
[](https://backlog.com/git-tutorial/tw/)
[](https://blog.darkthread.net/blog/category/Git/)
[](https://learngitbranching.js.org/?locale=zh_TW)
[](https://ithelp.ithome.com.tw/users/20004901/ironman/525)
[](https://git-scm.com/book/zh-tw/v2)
[](https://www.ruanyifeng.com/blog/developer/)

# Golang 筆記
##  Golang 的應用領域
* Block chain
* 遊戲、網站後端
* Cloud
## 為何google 創造Golang
* 目前主流的程式語言無法善用多核多cpu的優勢
* 軟體系統的複雜度越來越高，而缺乏足夠簡潔的的程式語言
## Golnag的特點
* 具有接近編譯型語言的運行速度，且有動態語言快速開發的優點
* 垃圾回收(記憶體自動回收)
* 自帶併發
  * CPS
* channel
* slice defer
## go run & build
* go build 類似c 先編譯後再運行
* go run 類似python直接運行
* 差別
  * go build 生成執行檔丟到沒有go開發環境的電腦依然可以運行(因為會把程式的依賴庫一起打包，理所當然的exe檔會變大)
  * go run 另外一台電腦也需要有go的開發環境，否則無法運行

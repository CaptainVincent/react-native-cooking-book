# Flux

[Reference](https://dotblogs.com.tw/blackie1019/2015/04/14/151049)

Flux 是由 Facebook 提出的一種 Design Pattern, 它不具有一個標準制式的框架, 而是一種設計的原則, 並已經有許多現存的實作模組, 可以簡化在系統中加入這樣架構的流程, 本範例使用的 Reflux 就是其中之ㄧ (但筆者目前查詢在 npm 上的排名已經不是最受歡迎的囉～, 想採用的可以再斟酌看看)。

### MVC 與遇到的問題
**簡單的 MVC 架構**

![](mvc-simple.png)

**複雜 MVC 可能出現的架構**

可以看到整個系統無限制的情況下, 會導致架構的發散與元件間互動關係的穿插複雜化, 最後導致於難以維護。

![](mvc-complex.png)

### Flux
Flux 的架構基本上可以視為一種抽象層的概念, 希望仍保有簡單 MVC 的元件單向互動模式。

![](mvc-Classic.png)

**簡易 Flux 架構**

![](flux-simple.png)

**複雜 Flux 架構**
![](flux-complex.png)

### Reflux
Reflux 的設計則是省略了 Dispatcher 的角色, Store 直接 listern Action。
> 筆者的觀點 Reflux 是仍舊保留 MVC 的架構, 但是要求資料流為單一方向, 所以更新父階層的狀態是透過發出 Action 來通知有在 Listen 的元件, 而非把父階層的函式直接傳遞給子代直接做 callback。

![](ZebretoReflux.png)


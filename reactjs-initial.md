# Reactjs 學習筆記

前話，其實學習 reactjs 不是單純在學習 reactjs 這個套件而已，其實，是結合了幾個重要的觀念和元素，包含 [reactjs][] , [jsx][] 和最重要的 [flux][] 概念，如果想要一次看懂，其實 [Jeremy Lu 的投影片][]，算是很好的概念入門投影片

## reactjs + jsx

對於 [jsx][] 來說，其實 [jsx][] 在 [reactjs][] ，最重要就是用作 VDOM 這個玩意，在開發時，傳統都是使用 template 概念，加入了 VDOM 後，可以使用跟 html / xml 概念相同的 markup language 開發，其實，某個面向來說，希望可以讓設計師和工程師之間的討論、誤解可以更少，有點私心的說法，期待設計師可以幫忙工程師寫一些 code 甚至入魔寫程式 \^\^

另一部分來說，其實使用 reactjs + [jsx][] ，其實可以完全脫離 html 的束縛，讓程式到模版，完全都只是在寫 js 就好了，不用這麼麻煩還要考慮現在到底在寫哪種 template 或是標籤有無結束的問題，只要有問題， js 的 debug 會相對簡單很多

題外話，使用 [reactjs][] + [jsx][] 還有一個很棒的好處，就是既然都是 js 產生的，其實是可以寫一次，前端、後端一次使用，只要善用像是 [browserify][] 或是 [webpack][]

## reactjs

### 生命週期

![react 生命週期][]
圖1.

資料參考自： [Component Specs and Lifecycle][]

其實 [reactjs][] 的生命週期循環是相當單純的，其實就如 圖1. 所示，與原本的 [MVC][] 相當不同，路徑只有單個方向，同樣的，也只是在操作生命週期的各個部分，避免可能造成的 [MVC][] 混亂

相關 [reactjs][] 的生命週期各個狀態說明如下：

#### render

**render** 所有 method 中最重要的一部分，也是***必須|不可或缺***的一個 method

其實可以把它解釋為**顯示**的概念，與 [nodejs][] 的 template 項目，都需要 render 概念是一模一樣的，差別只是在於，如過真的什麼都不要顯示，就直接讓 render 回傳 ***false*** 或是 ***null*** 就可以了

### getInitialState





## references

### hyperlinks
[reactjs]: https://facebook.github.io/react/index.html "reactjs"

[jsx]: https://jsx.github.io/ "jsx"

[flux]: http://facebook.github.io/flux/ "flux"

[Jeremy Lu 的投影片]: https://speakerdeck.com/coodoo/flux-in-action-shi-zhan-jing-yan-fen-xiang "Jeremy Lu 介紹 reactjs 的投影片"

[browserify]: http://browserify.org/ "browserify"

[webpack]: http://webpack.github.io/ "webpack"

[MVC]: http://zh.wikipedia.org/zh-tw/MVC "MVC"

[Component Specs and Lifecycle]: https://facebook.github.io/react/docs/component-specs.html "Component Specs and Lifecycle"

### images

[react 生命週期]: http://facebook.github.io/react/img/blog/flux-diagram.png "react 生命週期"
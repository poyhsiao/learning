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

類型：ReactElement

**render** 所有 method 中最重要的一部分，也是***必須|不可或缺***的一個 method

其實可以把它解釋為**顯示**的概念，與 [nodejs][] 的 template 項目，都需要 render 概念是一模一樣的，差別只是在於，如過真的什麼都不要顯示，就直接讓 render 回傳 ***false*** 或是 ***null*** 就可以了

#### getInitialState

類型：物件

在 ***元件被載入前***，只會觸發 ***一次***

回傳值會用在起始的 ***this.state*** 中

#### getDefaultProps

類型：物件

當物件被建立時，只會觸發 ***一次*** ，並且會 ***被加入快取中*** ，如果沒有設定 **父** 元件時，回傳值會被設定並對應到 ***this.props*** 中 (如，使用 ***in*** 檢查)

***getDefaultProps*** 會在實例建立前觸發，所此，沒辦法轉送 ***this.props*** ，此外，*請注意* ，所有複雜的物件，當使用 ***getDefaultProps()*** 時，都會在所有實例中分享並使用，不只是單純的複製而已 (Kim 註：類似 call-by-reference, js 物件共用概念)

#### propTypes

類型：物件

***propTypes*** 物件傳送到物件實作 props 驗證，可以參考 [Reusable Components][]

#### mixins

類型：陣列

***mixins*** 陣列供一個可以在許多元件中，共用 mixins 的行為，可以參考 [Reusable Components][]

#### statics

類型：物件

***statics*** 允許定義靜態的方法，也可以在物件中的各個元件中使用，例如：

```
var MyComponent = React.createClass({
    statics: {
        customMethod: function(foo) {
            return foo === 'bar';
        }
    },
    render: function() {
    }
});

MyComponent.customMethod('bar');  // true
```

在 ***statics*** 區塊中定義的方法，會在執行任何元件中建立，但是值得注意的是， ***statics*** 中的方法，沒辦法在任何元件中存取 props 或是 state，如果希望可以在 static 方法中，檢查 props 的值，必須使用 caller 方式傳遞 props 的值

(Kim註：也就是，只能把項目，使用 function call 的方式，傳入 statics 的方法中執行)

#### displayName

類型：字串

***displayName*** 用於除吋訊息中， JSX 會自動設定 ***displayName*** 的值。可以參考 [JSX in Depth][]

### 生命週期相關的方法

這些方法是指定元件於生命週期時間點的各種操作方式

#### Mounting: componentWillMount

```
componentWillMount()
```

對於 client 或是 server 端同樣有效，在出事元件呈現前會會觸發一次。如果在此一階段時有呼叫 ***setState*** ，render 會更新狀態並執行這 ***一次*** ，而且也不會在管狀態是否改變而再次觸發

#### Mounting: componentDidMount

```
componentDidMount()
```

當起始呈現物件後，只會觸發 ***一次*** ，而且，只對於 client 有效(Server 端無作用)，在生命週期的這個時間點，可以透過 ***React.findDOMNode(this)*** 存取這個 DOM 了。

如果希望整合其他 Javascript framework，例如時間控制 (***setTimeout*** 或是 ***setInterval***)，或是 ***AJAX*** 的項目需求，都可以在這個時間點來操作。

#### Updating: componentWillReceiveProps

```
componentWillReceiveProps(object nextProps)
```

當元件接收到新的 props 項目時觸發，這個方法無法在內部 render 時被呼叫。

使用 ***componentWillReceiveProps*** 的時機是用於 render 物件前，並且使用 ***this.setState()*** 方式更新 prop 的項目。原本的 props 可以使用 ***this.props*** 存取，在 ***componentWillReceiveProps()*** 方法中，使用 ***this.setState()*** 不會再次觸發其他的 render 功能。

```
componentWillReceiveProps: function(nextProps) {
    this.setState({
        likesIncreasing: nextProps.likeCount > this.props.likeCount
    });
}
```

> Note:
> 
> ***componentWillReceiveProps*** 與 ***componentWillReceiveState*** 不同，導入的 prop 會導致狀態的改變，但是相反的，這不是真實情況。如果需要在狀態改變時，如果希望操作狀態改變後的數值，則應該使用 ***componentWillUpdate***

#### Updating: shouldComponentUpdate




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

[Reusable Components]: https://facebook.github.io/react/docs/reusable-components.html "Reuseable Components"

[JSX in Depth]: https://facebook.github.io/react/docs/jsx-in-depth.html#the-transform "JSX in Depth"

### images

[react 生命週期]: http://facebook.github.io/react/img/blog/flux-diagram.png "react 生命週期"
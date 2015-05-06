# Reactjs 學習筆記

前話，其實學習 reactjs 不是單純在學習 reactjs 這個套件而已，其實，是結合了幾個重要的觀念和元素，包含 [reactjs][] , [jsx][] 和最重要的 [flux][] 概念，如果想要一次看懂，其實 [Jeremy Lu 的投影片][]，算是很好的概念入門投影片

## reactjs + jsx

對於 [jsx][] 來說，其實 [jsx][] 在 [reactjs][] ，最重要就是用作 VDOM 這個玩意，在開發時，傳統都是使用 template 概念，加入了 VDOM 後，可以使用跟 html / xml 概念相同的 markup language 開發，其實，某個面向來說，希望可以讓設計師和工程師之間的討論、誤解可以更少，有點私心的說法，期待設計師可以幫忙工程師寫一些 code 甚至入魔寫程式 \^\^

另一部分來說，其實使用 reactjs + [jsx][] ，其實可以完全脫離 html 的束縛，讓程式到模版，完全都只是在寫 js 就好了，不用這麼麻煩還要考慮現在到底在寫哪種 template 或是標籤有無結束的問題，只要有問題， js 的 debug 會相對簡單很多

題外話，使用 [reactjs][] + [jsx][] 還有一個很棒的好處，就是既然都是 js 產生的，其實是可以寫一次，前端、後端一次使用，只要善用像是 [browserify][] 或是 [webpack][]

## reactjs

### Top-level API

#### React

**React** 是進入 **React** 函式庫的的進入點，如果有使用其他以建立的套件，也可以把 **React** 設成全域變數。如果使用 ***[CommonJS][]*** 的方式管理模組，也可以使用 ***require()*** 的方式使用

##### React.Component

```
class Component
```

在定義 **[ES6][]** 的物件時， ***Component*** 是使用 **React Components** 最基礎的物件。可以參考 ***[Reusable Components][]*** 了解如何使用 **[ES6][]** 物件在 **React** 的使用方式，如果想要知道每個方法如何使用，可以參考 ***[Component API][]***

##### React.createClass

```
ReactClass createClass(object specification)
```

***createClass()*** 能夠建立一個元件的物件。一個元件使用 ***render*** 的回傳一個獨立的子項目，作為實作元件的方法。 子項目可以是隨意深度 (結構) 。與標準使用 ***prototype*** 方式建立的物件有所不同，並不需要使用 ***new*** 的方式產生新的物件，可以很方便地使用 ***createClass()*** 方式包裝新的實例 ( *instance* ) 使用。

如果想要知道更多關於物件操作方式，可以參考 [Component Specs and Livecycle][] \( Kim 註： [React 生命週期](#React_life_cycle) )

##### React.createElement

```
ReactElement createElement(
    string/ReactClass type,
    [object props],
    [children ...]
)
```

根據所指定的類型，產生並回傳 **ReactElement** 。類型的參數可以是 html 標籤，或是標籤名稱 (如： 'div', 'span' 等)，或是 **ReactClass** (透過 ***React.createClass*** 所產生的)

##### React.cloneElement

```
ReactElement cloneElement(
    ReactElement element,
    [object props],
    [children ...]
)
```

運用克隆 (Kim 註：完整複製) 和回傳一個新的 **ReactElement** 元件。新產生的元件會具備有與原本的元件有著相同的 **props** 。新的子元件，會取代原本已經存在的子元件。不同於 ***React.addons.cloneWithProps*** ，相對的 *key* 和 *ref* 會完整被保留，而且在整合任何 **props** 後，也不會有特別的行為發生 (不像 **cloneWithProps** )，可以參考 [v0.13 RC2 blog post][] ，以了解完整的差異。

##### React.createFactory

```
factoryFunction createFactory(
    string/ReactClass type
)
```

根據所給定的 **ReactElements** 的類型，回傳一個 *function* ，類似於 ***React.createElement*** ，類型可以是 html 標籤字串 (如：'div', 'span' 等)，或是 **ReactClass**

##### React.render

```
ReactComponent render(
    ReactElement element,
    DOMElement container,
    [function callback]
)
```

在 DOM 指定的內容中，呈現 **ReactElement** ，並且回傳相關的元件。

如果 **ReactElement** 已經在指定的內容中呈現了，呼叫 ***render*** 則會更新原本已經存在於 DOM 中的元件，並且更新 *React* 的元件。

如果有提供 **callback function** ， **callback function** 會在網頁元件呈現或更新後被呼叫。

> Note:
> 
> ***React.render()*** 會取代所提供網頁元件的內容項目。未來， ***React.render()*** *可能* 只會更新已經存在的 DOM 項目，而不是覆蓋已存在的子元件。

##### React.unmountComponentAtNode

```
boolean unmountComponentAtNode(DOMElement container)
```

移除一個已經在 html DOM 呈現的 React 元件，並且會清除被綁定的事件處理器以及其生命週期的 state。如果目前沒有任何元件存在於指定的容器中， ***unmountComponentAtNode()*** 不會做任何事。如果成功移除 html 元件，則會回傳 *true* ，如果容器內沒有任何元件，則會回傳 *false*

##### React.renderToString

```
string renderToString(ReactElement element)
```

運用於 **ReactElement** 產生出初始的 html 時。 ***renderToString()*** 應該只能使用在 *server* 端使用。 React 會回傳一串的 html 字串。可以使用 ***renderToString()*** 在 *server* 端產生 html 以及快速產生需要的初始標記語言。這對於搜尋引擎 (*[SEO][]*) 已抓取網頁方面，有著很棒很顯著的效果。

如果在 server 端，已經產出 html 的環境中呼叫 ***React.render()*** ， React 會保存已產出的 html 標記語言，以及只加入事件處理項目。這對於增加第一次頁面讀取的表現以及使用者體驗有很大的助益。

##### React.renderToStaticMarkup

```
string renderToStaticMarkup(ReactElement element)
```

類似於 ***renderToString*** ，差別在於 ***renderToStaticMarkup*** 不會產生 DOM 的屬性 (如： **data-react-id** )(Kim 註： 只會產生乾淨的 html，而沒有 React 專用的 id)。這用於使用 **React** 產生非常乾淨簡單的靜態頁面非常有用，而且也不會有其他的屬性，相對也會節省許多空間/容量。

##### React.isValidElement

```
boolean isValidElement(* object)
```

驗證物件是否是 *ReactElement* 。

##### React.findDOMNode

```
DOMElement findDOMNode(ReactComponent component)
```




### <a name="React_life_cycle"></a>React生命週期

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

```
boolean shouldComponentUpdate(object nextProps, object nextState)
```

當新的 props 或是新的 state 接收到，而且在顯示之前會觸發。 ***shouldComponentUpdate*** 在初始顯示前無法觸發或是呼叫， ***forceUpdate*** 也無法觸發。

在新的 props 和 stats 發生改變時，使用 ***shouldComponentUpdate*** 會回傳 **false**，也不需要元件確實更新。

```
boolean shouldComponentUpdate(object nextProps, object nextState) {
    return nextProps.id !== this.props.id;
}
```

如果 ***shouldComponentUpdate*** 回傳 **false**， ***render()*** 會完全忽略直至下個 state 改變發生的時候 (此外， ***componentWillUpdate*** 以及 ***componentDidUpdate*** 都不會被觸發或是執行)

為了要避免在狀態改變的情況下可能的錯誤發生，在預設情況下， ***shouldComponentUpdate*** 通常都回傳 **true** ，但是，如果你夠小心，讓狀態都保持在固定且唯讀 props 處於固定的狀態下，則你可以不理會 ***shouldCompoenentUpdate*** ，則可以比較前後 props 以及 state 的部份。

如果效能有瓶頸，尤其是有非常多的元件時，使用 ***shouldComponentUpdate*** 可以加速你的程式

#### Updating: componentWillUpdate

```
componentWillUpdate(object nextProps, object nextState)
```

當收到新的 props 或是 state ，並且在產出元件前，將會立刻被觸發。 ***componentWillUpdate*** 不會在初始建立元件時被觸發。

使用時機在於更新發生前觸發。

> Note:
> 
> 不能在 ***this.setState()*** 使用 ***componentWillUpdate***。如果需要更新在 prop 改變時，更新 state ，則應該使用 ***componentWillReceiveProps***

#### Updating: componentDidUpdate

```
componentDidUpdate(object prevProp, object prevState)
```

在元件更新而且產出 DOM 後，則立即會觸發。 ***componentDidUpdate*** 無法在初始產出元件時觸發或呼叫。

使用時機在元件被更新且 DOM 被更新時。

#### Unmounting: componentWillUnmount

```
componentWillUnmount()
```

在元件從 DOM 中移除前，立即會觸發。

在個別的計時器 (timers) 或是清除任何 DOM 元件時，會先觸發 ***componentDidMount*** ，會清除 ***componentWillUnmount*** 方式的作用

## references

### hyperlinks

[CommonJS]: http://en.wikipedia.org/wiki/CommonJS "CommonJS"

[ES6]: http://www.codedata.com.tw/javascript/introducing-es6-1-harmony-history "ECMAScript 6"

[Component API]: https://facebook.github.io/react/docs/component-api.html "Component API"

[v0.13 RC2 blog post]: https://facebook.github.io/react/blog/2015/03/03/react-v0.13-rc2.html "v0.13 RC2 blog post"

[Component Specs and Livecycle]: https://facebook.github.io/react/docs/component-specs.html "Component Specs and Livecycle"

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
---
layout: post
title: "Container Components"
img: react.png # Add image post (optional)
date: 2018-02-26
description: Container component pattern # Add post description (optional)
tag: [React, Redux, JavaScript]
---
# 容器組件模式

有一個React的模式對我影響很大，它就是*容器組件模式（Container component pattern）*。

在 Jason Bonta的高性能組件研討會中提及到此， [這個是鏈接](https://www.youtube.com/watch?v=KYzlpRvWZ6c&t=1351)。



這個概念很簡單：

***容器組件負責數據的提取，然後渲染其相應的子組件。就這樣。***

*“相應的子組件”*的意思是指有共享相同名字的組件，舉個例子：

```javascript
StockWidgetContainer => StockWidget
TagCloudContainer => TagCloud
PartyPooperListContainer => PartyPooperList
```

現在你應該很好能夠理解這些概念了。

# 為什麼要需要容器組件？

假設你有一個顯示評論的組件，而且你不認識容器組件這個模式。自然而然，你把全部東西放到一個籃子里了。

```react
class CommentList extends React.Component { 
  this.state = {comments：[]}; 

  componentDidMount（）{ 
    fetchSomeComments（comments => 
      this.setState（{comments：comments}））; 
  }
  render（）{ 
    return（
      <ul> 
        {this.state.comments.map（c =>（
          <li> {c.body}  -  {c.author} </li> 
        ））} 
      </ul> 
    ）; 
  } 
}
```

這個組件負責提取數據和渲染。看起來并沒有什麼“錯誤”，但是你錯過了React的最大的好處。



#### 複用性（Reusability）

**CommentList**不能重複使用，除非在完全相同的情況下。



#### 數據結構

這個makeup組件應該要聲明它所需的預期數據，PropTypes可以很好解決這個問題。

我們上面那個組件，對數據結構的耦合性很強。如果API的改變，這個組件將會失效。



## 使用容器模式的實踐

首先，先把數據提取到容器組件：

```react
class CommentListContainer extends React.Component {
  state = { comments: [] };
  componentDidMount() {
    fetchSomeComments(comments =>
      this.setState({ comments: comments }));
  }
  render() {
    return <CommentList comments={this.state.comments} />;
  }
}
```

現在，重構CommentList，我們用props來接收comments的數組。

```react
const CommentList = props =>
  <ul>
    {props.comments.map(c => (
      <li>{c.body}—{c.author}</li>
    ))}
  </ul>
```



## 那麼，我們從這個模式得到了什麼？

我們實際上得到了很多很多東西....

首先，我們分離了我們的**數據提取(data-fetching)**和**渲染(rendering)**。

我們使得我們的CommentList組件更具**複用性(reusable)**。

我們給予了CommentList 有能力去設置 PropTypes和容錯能力提高了。

我是一個容器組件模式的忠實粉絲，它可以把我的代碼的容讀性提高，架構更清晰。你自己也可以嘗試這個模式，還有請看看[Jason的研討會](https://www.youtube.com/watch?v=KYzlpRvWZ6c).，這太贊了。

Carry on, nerds.

--------------------------------------------------

轉載于[Medium](https://medium.com/@learnreact/container-components-c0e67432e005)

翻譯得不好，錯的東西多多包涵，請留意指出錯誤。
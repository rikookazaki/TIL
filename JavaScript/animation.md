### 画面が読み込まれると、指定の要素がふわっと浮かび上がってくるように現れるようにする方法  
例：contentというクラス名の要素を浮かび上がらせる  
CSS
```
.content{
  opacity: 0;
  transform: translateY(20px)
}

.fade-in{
  opacity: 1;
  transform: translateY(0);
  transition: opacity 1s ease, transform 1s ease;
}
```

js
```
function transform() {
  const contentElements = document.querySelectorAll(".content");
  contentElements.forEach(element => {
    element.classList.remove("fade-in");
    
      element.classList.add("fade-in");
    }, 10);  
  });
}

document.addEventListener("DOMContentLoaded", transform);
document.addEventListener("turbo:load", transform);
```

### 解説
* contentクラスは、初期状態では`opacity: 0;//完全に透明`且つ、  
`transform: translateY(20px)//指定の場所から下方向に20pxの位置にある`
* jsの記述により、ページが読み込まれると、関数transformが発動、contentクラスを持つ要素には、fade-inクラスが追加される
* fade-inクラスのもつCSSにより、
  `opacity: 1;//不透明`
  `transform: translateY(0);//元の位置に戻る`
  `transition: opacity 1s ease, transform 1s ease;//上記の2点が1秒かけて行われる`
  となり、ふわっと浮かび上がるように見える。

### 補足
リンクやブラウザの戻るボタンなどでページが遷移された場合、turboの働きにより、変更のない点は読み込まれない。  
そのため、  
```
function transform() {
  const contentElements = document.querySelectorAll(".content");
  contentElements.forEach(element => {
      element.classList.add("fade-in");
    });  
   }

document.addEventListener("DOMContentLoaded", transform);
document.addEventListener("turbo:load", transform);

```
このような記述だと、"fade-in"クラスがすでにcontentに適応されている状態で、ページが遷移されても変化が見られない。  
一度"fade-in"クラスを消してリセットしたのち、再びクラスを加えるという作業が必要である。

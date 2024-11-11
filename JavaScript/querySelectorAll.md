### 要素の取得　~querySelectorAll~
querySelectorAllでは、同じクラスなどを持つ複数の要素に対して、一度にjsの処理を適応させることができる。  
例："btn"というクラス名を持つ複数の要素に、カーソルを合わせると背景色が赤に変わるという処理をつける  
```
const button = document.querySelectorAll(".btn");
button.forEach(function(btn) {
    btn.addEventListener('mouseover', function() {
      btn.setAttribute("style", "color:#red;");
    });
    btn.addEventListener('mouseout', function() {
      btn.removeAttribute("style");
    });
  });
```

### 解説
* まず、`const button = document.querySelectorAll(".btn");`で、"btn"というクラス名を持つ複数の要素を全て取得する。
* forEachを用いて、取得した要素全てに対して
  `mouseover`のイベント発火で`setAttribute("style", "color:#red;");`、  
  `mouseout`のイベント発火で`removeAttribute("style")`が適応される。

### ポイント
* querySelectorAllはセレクタを用いて要素を取得する。クラス名からの場合は`".クラス名"`の形になる。
* 複数の要素に対してそれぞれ処理を加えたい場合は、forEachを用いる。
  `forEach(function(ブロック変数){`という記述で要素の全てに処理を適応できる。

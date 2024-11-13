### seedとは
テーブルに初期値としてデータを設定することができる。  
`db/seeds.rb`ファイルに記述する  
#### seedの反映方法
* 最初に記述したとき  
`% rails db:seed`  
* 修正を反映させたい時  
`% rails db:seed:replant  //一度seedをリセットしてから現在のseed.rbを読み込む`

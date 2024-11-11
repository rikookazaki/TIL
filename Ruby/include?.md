### include?メソッド
指定した値が配列や文字列内に含まれているかを判定するメソッド。  
含まれている場合はtrueを、含まれていない場合はfalseを返り値として返す。  

配列の判定：  
```
array = ["foo", "bar"]
puts array.include?("bar")
# => true
puts array.include?("hoge")
# => false
```

文字列の判定：
```
"hello".include? "lo"
# => true
"hello".include? "ol"
# => false
```

#### テーブルにカラムを追加
```rails generate migration Addカラム名Toテーブル名 カラム名:カラムの型```  

例：usersテーブルに、nicknameカラムをstring型で追加  
```rails generate migration AddNicknameToUsers nickname:string```

#### テーブルからカラムを削除
```rails generate migration Removeカラム名Fromテーブル名 カラム名:カラムの型```  

例：usersテーブルの、string型nicknameカラムを削除  
```rails generate migration RemoveNicknameToUsers nickname:string```

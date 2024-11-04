
# Ransack　複数条件での検索
```
def index
    @q = Restaurant.ransack(params[:q])
    @restaurants = @q.result.includes(:countries, :genres, :situations)

    if params[:q].present?
      # 国名によるフィルタリング
      if params[:q][:countries_name_or_countries_region_cont].present?
        @restaurants = @restaurants.joins(:countries)
          .where(countries: { name: params[:q][:countries_name_or_countries_region_cont] })
      end

      # ジャンルによるフィルタリング
      if params[:q][:genre_ids_in].present?
        @restaurants = @restaurants.joins(:genres)
          .where(genres: { id: params[:q][:genre_ids_in] })
      end

      # 利用シーンによるフィルタリング
      if params[:q][:situation_ids_in].present?
        @restaurants = @restaurants.joins(:situations)
          .where(situations: { id: params[:q][:situation_ids_in] })
      end
    end

    # 条件を満たす飲食店を表示
    @restaurants = @restaurants.distinct
  end
```

解説
1. `@q = Restaurant.ransack(params[:q])`
* 何をしているか: `params[:q]`に渡された検索条件をもとに、Restaurantモデルに対するRansackの検索オブジェクトを作成します。
* 背景: Ransackは、検索機能を簡単に実装できるライブラリで、パラメータを指定することで複雑なクエリを生成できます。

2. `@restaurants = @q.result.includes(:countries, :genres, :situations)`
* 何をしているか: @q.resultを呼び出すことで、指定された検索条件に基づいて、関連するcountries、genres、situationsを事前に読み込んだ状態で飲食店のデータを取得します。
* 背景: includesメソッドはN+1問題を避けるために使われ、関連するテーブルのデータを事前に取得します。

3. 検索条件の確認とフィルタリング
    この部分は、ユーザーが入力した検索条件に基づいて、@restaurantsをさらに絞り込みます。
   
   ```if params[:q].present?```
   
   何をしているか: 検索条件が存在する場合にのみ、以下のフィルタリング処理を実行します。

    a. 国名によるフィルタリング
    ```
    if params[:q][:countries_name_or_countries_region_cont].present?
        @restaurants = @restaurants.joins(:countries)
          .where(countries: { name: params[:q][:countries_name_or_countries_region_cont] })
      end
    ```
    
   何をしているか: 国名または地域名の検索条件が存在する場合、countriesテーブルに結合し、指定された国名に一致す.  る飲食店を取得します。

     b. ジャンルによるフィルタリング
   ```
     if params[:q][:genre_ids_in].present?
       @restaurants = @restaurants.joins(:genres)
          .where(genres: { id: params[:q][:genre_ids_in] })
     end
   ```
   
   何をしているか: ジャンルの検索条件が存在する場合、genresテーブルに結合し、指定されたジャンルIDに一致する飲食店を取得します。

      c. 利用シーンによるフィルタリング
   ```
       if params[:q][:situation_ids_in].present?
         @restaurants = @restaurants.joins(:situations)
            .where(situations: { id: params[:q][:situation_ids_in] })
       end
   ```
   
   何をしているか: 利用シーンの検索条件が存在する場合、situationsテーブルに結合し、指定されたシーンIDに一致する飲食店を取得します。


5. `@restaurants = @restaurants.distinct`
* 何をしているか: 最後に、条件に一致した飲食店の中から重複を取り除きます。
* 背景: distinctを使うことで、同じ飲食店が複数回結果に表示されるのを防ぎます


3が必要な理由
* `@q = Restaurant.ransack(params[:q])` と `@restaurants = @q.result.includes(:countries, :genres, :situations)` だけでは、検索条件に基づいたフィルタリングが十分には行われません。このコードは、Ransackを使って検索オブジェクトを作成し、関連するテーブル（countries, genres, situations）を含めて結果を取得するための基本的な処理を行っていますが、具体的な条件による絞り込みは行っていません。

* 具体的なフィルタリングがない: @q.result は、Ransackが提供する条件に基づいて飲食店を絞り込むものではありますが、実際の条件（国名、ジャンル、利用シーンなど）に基づいて飲食店をフィルタリングするための joins や where 条件が必要です。これらの条件を追加することで、ユーザーが入力した検索条件に正確に応じた結果を得ることができます。
単純な検索条件の取得: Ransackは単純な検索条件（例えば、nameやaddressなど）に対しては直接フィルタリングできますが、複数のテーブルとの関係がある場合（この場合は countries, genres, situations との多対多の関係）では、joins と where を使って明示的に条件を指定しないと、正しい結果を得ることができません。


* このコードがあることで、ユーザーの入力に応じた適切な飲食店を表示できるため、検索機能が実際に役立つものになります。もしこの部分を削除すると、検索機能は単なる一覧表示に過ぎなくなります。





# Join  whereについて

joins メソッド
関連するテーブルを結合するために使用されます。これにより、関連するデータを同時に取得でき、特定の条件で絞り込むことができます。
使用例:
```
* RestaurantとCountryを結合して、特定の国に関連するレストランを取得
@restaurants = Restaurant.joins(:countries).where(countries: { name: '日本'})
```

where メソッド
特定の条件に基づいてデータをフィルタリングするために使用されます。条件は、ハッシュ形式またはSQL文として指定できます。
使用例:
```
* 特定のジャンルに関連するレストランを取得
@restaurants = Restaurant.joins(:genres).where(genres: { id: 1 })
```


* joins と where の組み合わせ
これらのメソッドはよく組み合わせて使われます。例えば、特定の国に属し、特定のジャンルに関連するレストランを取得したい場合は、以下のようにします。
```
@restaurants = Restaurant.joins(:countries, :genres)
                         .where(countries: { name: '日本' })
                         .where(genres: { id: 1 })
```

この例では、joins によって countries と genres の両方を結合し、その後 where を使ってそれぞれの条件を指定しています。これにより、国が「日本」で、ジャンルIDが1のレストランだけを取得します。


## MovieLens 1M データに対してリコメンドアルゴリズムの選択
### データセットの紹介と探検
データセット：MovieLens 1M  
このデータセットは6040人のユーザーが3952本の映画に対して1000209個の評価記録を保存しています。  
[MovieLens_data_analysis](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/MovieLens_data_analysis.ipynb)はデータセットの探検  

### リコメンドアルゴリズムの実装
[MovieLens_recommend](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/MovieLens_recommend.ipynb)で三つのリコメンドの実装と評価を行っています。
#### 1.　アイテムに基づくリコメンド
アイテムに基づくリコメンドにおけるアイテム間の類似性は、2つのアイテム間でスコア付けてすべてのユーザーを観察することによって測定されます。各アイテムｉとアイテムｊとの間の類似度を計算することによって、n*nの類似度行列を得ることができます。
類似度の距離はcosine距離を使います。　　
![image](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/image/item.png)  
ユーザｉによる全アイテムの評価が重みとして、アイテムjとの類似度と掛けで、アイテムjに対するユーザーiの評価予測を得る。  

#### 2. ユーザーに基づくリコメンド
現在のユーザーに似ているn人のユーザーを見つけ、k人のユーザーが好みまたは現在のユーザーに知られていないアイテムを現在のユーザーに推薦します。
類似度行列の計算とアイテムに基づくリコメンド同じです。　　

#### 3. SVDに基づくリコメンド
評価行列は、ユーザの好みのノイズにオーバーフィットの可能性があります。したがって、SVDで次元削減は不要なノイズを削除できとデータの中に隠された相関/特徴を発見します。  
SVDは既存の評価状況に基づいて各アイテムに対するユーザーの好みとアイテムの各因子を含む程度を分析し、分析結果に基づいて最終的に評価を予測することです。具体的にはm*nのランキング行列Aをm\*rのユーザー行列Uとr\*nのアイテム行列Vに分解します。行列∑は特異値の対角行列です。  
Aの要素が大きいほど、ユーザーはアイテムを好きになります。 Uの要素が大きいほど、ユーザーは対応する因子を好みます。 Vの要素が大きいほど、アイテムと因子対応の程度が大きくなります。  
より低いランクの近似を得るために、行列を取り、トップkの特徴だけを残します。(ここでｋ=20)
![image](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/image/svd01.png)
具体的にuser_id=1000のユーザーの推薦ランキング(TOP5)を作ります：
![image](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/image/svd_output.png)  

#### 4. モデルの評価
評価指標：Root Mean Square Error (RMSE)  
![image](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/image/RMSE.png)  
各リコメンドアルゴリズムにおいて訓練データとテストデータを割る、訓練データとテストデータに対してRMSEは以下に示します：
![image](https://github.com/Jellytrial/MovieLens-Recommend/blob/master/image/evl.png)

### まとめ
* 全体として、小規模のMovieLens 1Mデータセットに基づくと、この場合の3つのリコメンドアルゴリズムは次のように表すことができます：リコメンドアルゴリズム予測効果：SVD>User based>Itembased
* ユーザーに基づくリコメンドから、リコメンドのデータ数が多く、またデータ間のインタラクティブが多ければ、リコメンド効果は良くなる。しかしながら、インタラクティブは足りないからリコメンドシステムが必要です。したがって、できるだけ多くのデータを収集することは重要と思います。
* 3つのリコメンドアルゴリズムのうち、SVDリコメンドアルゴリズムは優れており、予測結果は他の2つのリコメンドアルゴリズムと比較して改善します。

### 参考資料
1. [Simple Movie Recommender Using SVD](https://alyssaq.github.io/2015/20150426-simple-movie-recommender-using-svd/)
2. Kalman D. A singularly valuable decomposition: the SVD of a matrix[J]. The college mathematics journal, 1996, 27(1): 2-23.
3. [推荐系统之协同过滤（CF）算法详解和实现](https://www.cnblogs.com/maybe2030/p/4636341.html)

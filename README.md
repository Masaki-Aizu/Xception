# Xception
## ポイント
- 3x3 convの数自体は増えるが、1x1 convでチャンネルを1に圧縮し、それぞれのconvの計算量が激減させている
- この時、もはや3x3 convは1チャンネルしか入力されていないので、**チャンネル同士の関係性を見ることはない**
<img alt="Xception module" src=./image/xception_module.avif></img>
- Residual connectionによって、精度も高く、さらに収束もより早くなっている
- **このモジュールはXeception仮説に基づいて作成されている**
- Xception仮説に基づいてXception ModelをGoogLeNetと同じくらいのパラメータ数で構築したら圧勝したというのが理論的バックグラウンド
- InceptionV3の方が、ImageNetにおける精度が高かった。これはInceptionV3がImageNetに重点を置いて開発されており、設計上、この特定のタスクに過剰に適合している可能性があるためであると考えられる
## Xception仮説
- channel correlation（チャンネル方向の相関）とspacial correlation（空間方向の相関）が完全に分離可能という仮説
# Xception Module
- Depthwise separable convolutionにほぼ同じ
- チャンネルごとに独立した空間方向convolutionとPointwise (1x1) convolutionのセット
- Xception = pointwise conv + spacial conv　となる
- 補足：Depthwise separable conv = spacial conv + pointwise conv（MobileNetに使用される構造）:入力チャンネルそれぞれにN×Nの畳み込みを行い、その後その出力をまとめた特徴量マップに1×1（チャンネル方向）の畳み込みを行う
## アーキテクチャ 
<img alt="Xception" src=./image/Xception.avif></img>
## 学習方法
#### On ImageNet:
– Optimizer: SGD
– Momentum: 0.9
– Initial learning rate: 0.045
– Learning rate decay: decay of rate 0.94 every 2 epochs
- データオーギュメンテーションは不明
#### On JFT:
– Optimizer: RMSprop 
– Momentum: 0.9
– Initial learning rate: 0.001
- Learning rate decay: decay of rate 0.9 every 3,000,000 samples
- データオーギュメンテーションは不明
## テスト推論
- 1つのモデルに画像のクロップを入力し、精度計算
## 参考
1. https://qiita.com/woodyZootopia/items/3adc613e7717b6b5a260
2. https://arxiv.org/pdf/1610.02357.pdf
##　※畳み込みまとめ
- 畳み込み層は、2 つの空間次元 (幅と高さ) とチャネル次元を持つ 3D 空間でフィルターを学習しようとする。したがって、通常の畳み込み層は、**チャネル間相関と空間相関を同時にマッピングする役割を果たす**
- インセプション モジュールは、まず **1x1 畳み込みを介してクロスチャネル相関を調べ、入力データを元の入力空間チャンネルよりも小さい3つ、または4つの個別の空間にマッピングし、次に、すべての相関をこれらの小さな 3D 空間にマッピングする**
- インセプションの背後にある仮説は、特徴量マップはクロスチャネル相関と空間相関が分離されていることである（言い換えれば同じニューロンを持つ出力をまとめるということ）

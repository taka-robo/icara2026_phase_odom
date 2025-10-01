# 論文アウトライン（サンプル）: 深層学習を用いた画像分類の実験的評価

## 0. メタ情報
- ターゲットカンファレンス: 国内機械学習ワークショップ（6〜8 ページ想定）
- 執筆言語: 日本語（英語タイトル・参考文献）
- スタイル候補: `template/IEEEtran.cls` 由来の 2 カラムフォーマット
- キーワード: 画像分類, 深層学習, ResNet-18, Vision Transformer, Test-Time Augmentation

## 1. 概要（Abstract）
- 研究背景と課題設定を 3〜4 文で説明
- 提案する比較プロトコルの特徴と再現性確保の工夫を記述
- 主な実験結果（ViT-Ti が TTA で +1.8pt、ResNet-18 はデータ拡張で +2.1pt）を定量的に示す

## 2. 研究課題と貢献
- 課題: 軽量 CNN と ViT を一貫した条件で比較可能なベースラインの欠如
- 貢献1: CIFAR-10 を用いた最小構成の比較枠組みを設計（
  - モデル設定: ResNet-18 \cite{he2016deep} と DeiT-Ti \cite{touvron2021training}
  - 最適化: SGD + Cosine Annealing, AdamW + Warmup の統一スキーム
- 貢献2: データ拡張（RandAugment \cite{cubuk2020randaugment}）と TTA の効果をベンチマーク
- 貢献3: 5 回の再現実験で分散と平均を報告し、軽量モデルでも堅牢さを確保

## 3. セクション構成

### 第1章 はじめに
- 画像分類の発展と応用領域を概観
- 再現性問題・設定の不統一性に触れ、比較研究の必要性を指摘 \cite{henderson2018deep}
- 本稿の目的と貢献、構成の案内

### 第2章 関連研究
- CNN 系モデルの進化: AlexNet \cite{krizhevsky2012imagenet}, VGG \cite{simonyan2014very}, ResNet 系の展開
- Transformer 系モデル: ViT \cite{dosovitskiy2021an}, DeiT \cite{touvron2021training} の軽量化アプローチ
- 学習を支援する手法: 学習率スケジューリング \cite{loshchilov2017sgdr}, 正則化手法（Dropout, Label Smoothing \cite{muller2019does}, RandAugment）
- 既存比較研究の課題（評価設定のばらつき、計算コスト）

### 第3章 手法と実験設定
- モデル構成
  - ResNet-18: 11.7M パラメータ、BatchNorm の役割
  - DeiT-Ti: 5.7M パラメータ、Multi-Head Attention の概要
- データセットと前処理
  - CIFAR-10 の統計、標準化手順、Cutout/TTA 条件
- 学習プロトコル
  - 共通ハイパーパラメータ（エポック 90、バッチサイズ 128）
  - CNN: SGD + Momentum、学習率 0.1 → Cosine Annealing
  - ViT: AdamW、学習率 5e-4、線形ウォームアップ 5 エポック
- 評価指標
  - Top-1 精度、ロス、推論時間、パラメータ数
  - TTA 設定（水平反転、10-crop 評価）

### 第4章 実験結果
- 学習曲線: 損失・精度の推移を図示（Fig.1）
- エポック 90 時点の精度比較を表形式で提示（Table 1）
- TTA 有無での精度差と推論時間のトレードオフ（Table 2）
- 複数シードでの平均と標準偏差（Table 3）

### 第5章 考察
- CNN と ViT の挙動差（初期収束と最終精度）
- データ拡張の効果: CNN では大きいが ViT では飽和気味
- TTA の実運用上の負荷と得られる性能向上のバランス
- 限界: CIFAR-10 のみ、軽量モデルに限定、計算資源

### 第6章 結論
- 研究のサマリーと主要知見の再掲
- 実務応用: 軽量モバイル環境でのモデル選定ガイドライン
- 今後の課題: 大規模データセット（ImageNet-1k）や蒸留手法の検証

### 謝辞
- オープンソースコミュニティ、使用したライブラリ（PyTorch 等）への謝辞

### 参考文献候補
- \cite{krizhevsky2012imagenet} AlexNet
- \cite{simonyan2014very} VGG
- \cite{he2016deep} ResNet
- \cite{dosovitskiy2021an} ViT
- \cite{touvron2021training} DeiT
- \cite{loshchilov2017sgdr} SGDR / Cosine Annealing
- \cite{muller2019does} Label Smoothing
- \cite{cubuk2020randaugment} RandAugment
- \cite{henderson2018deep} 再現性

## 図表アイデア
- Fig.1: 学習曲線（ResNet vs DeiT）
- Fig.2: 適用したデータ拡張のイメージ
- Table 1: ベースライン精度比較
- Table 2: TTA 効果と推論時間
- Table 3: シード別統計

## 作業メモ
- 図表データは `out/figures/` に配置予定
- 実験ログは `project/results/` に保存し、必要に応じて Appendix 化
- 参考文献の BibTeX は `project/references.bib` に集約予定

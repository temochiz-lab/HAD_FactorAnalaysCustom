1.Rのダウンロード
https://cran.rstudio.com/bin/windows/base/

2.R Studioのダウンロード
https://posit.co/download/rstudio-desktop/#download

3.R Markdownの有効化
File -> NewFile -> R Markdown
(自動でダウンロード、インストールされる)

4.ggplotのインストール
install.packages("tidyverse")
library(tidyverse)

====================================
因子分析
====================================
# (1) ファイルの読み込み
w1 <- read.csv("C:/Users/temoc/Desktop/seiketsu.csv")

# 数値データのみを選択
w1 <- w1[sapply(w1, is.numeric)]

# (2) 相関を計算
w2 <- cor(w1)
y <- eigen(w2)$values
x <- 1:length(y)

# (3) スクリープロットの作成
library(tidyverse)

# プロットを確認
ggplot() + 
  geom_point(aes(x = x, y = y)) + 
  geom_line(aes(x = x, y = y)) + 
  geom_hline(yintercept = 1)

# (4) 因子分析
fit <- factanal(w1, factors = 3, rotation = "promax")


# 因子負荷行列の抽出
loadings <- fit$loadings

# 因子負荷行列をデータフレームに変換
loadings_df <- as.data.frame(as.table(loadings))

# データフレームの確認
print(head(loadings_df))

# 各因子の負荷量をVar2でソートし、次にFreqの絶対値でソート
sorted_loadings <- loadings_df %>%
  arrange(Var2, desc(abs(Freq)))

# Factor1, Factor2, Factor3 すべてで Freq が 0.40 以下の行を抽出
filtered_loadings <- sorted_loadings %>%
  group_by(Var1) %>%
  filter(all(abs(Freq) <= 0.40)) %>%
  ungroup()

# 抽出された行の表示
print("Filtered Loadings (All Freq <= 0.40):")
print(filtered_loadings)

--------------------------------------
評価

SPSS
　スタンダードにして最高級品。　
　とは言え、グリッドの編集は機能の貧弱さを感じる。
　改変はできない。
PSPP
　SPSSになりたいフリーソフト。　ライセンスはGPL。
　使い勝手は決して良くなく、機能面でも不足を多く感じる。
　コードの公開をするのであれば改変OK、しかし Classic な C で書いたコードで頑張りたくない。
HAD
　清水先生作のExcelマクロ。　なのでExcelを購入していなければ使用できず、Libre Office や Google Sheets では利用できない。
　当大学の場合は、学生に Office の利用権があるので費用はかからないが、周知が足りずみんな知らなかった。
　Excel が母体なので、入力や加工にExcelの全機能を利用できて非常に便利。
　問題1: 運営が清水先生個人ぽい
　　・サーバは、誰によって維持管理しているのか謎で、一応自分でも持っておかないと不安。　せめて git に置いてほしい。
　　・清水先生にもしもの事があった場合に、共同運営者がいて継続できるのか、それとも終わってしまうのかが不明、これが最大の懸念。
　問題2: ライセンス形態が不明である。　このため、
　　・自分用にカスタマイズした版を作成・配布して良いのか不明。
　　・Libre Office , Google Sheets に対応させたバージョンを作成・配布して良いのか不明。
R
　こちらもスタンダードではあるが、GUIは期待できず、プログラムを書ける人向け。
　逆に、プログラムで書いておいた方が、GUIの操作手順をおぼえるよりも楽。
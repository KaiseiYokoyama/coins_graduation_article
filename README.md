# 筑波大学情報科学類卒業論文テンプレート + CI
## 使い方
1. このリポジトリをForkします
2. ForkしたリポジトリをCloneし，ローカルで卒論を書きます．
3. PDFを生成したいcommitに`v`から始まる名前のタグをつけ，***タグを***Pushします
4. Github Actionsが作動し，リリースページのAssetsにPDF`graduation_article_(タグ名).pdf`が並びます

## カスタマイズ
### `.github/workflows/release.yml`
Github Actionsの動作を定義するファイルです．Actionsをどのタイミングでトリガするのか，作成するリリースの名前，出力されるPDFの名前などが設定できます．

また，コンパイルする`.tex`ファイルの名前やlatexのコンパイラ，コンパイル時の引数，`tlmgr`経由で追加するtexパッケージの名前などが指定できます．

latexまわりの挙動については，[こちら](https://github.com/KaiseiYokoyama/latex-build-langja)をご覧ください．

### `.latexmkrc`
latexを`latexmk`を使ってコンパイルする際のあれこれを設定するファイルです．

## その他
### tag名（バージョン名）をlatexの文書中で参照したい
Github Actionsの動作中，環境変数 `release_version` が定義されています．なので，例えばプリアンブルなどで，

```latex
\newcommand{\getenv}[2][]{%
  \CatchFileEdef{\temp}{"|kpsewhich --var-value #2"}{\endlinechar=-1}%
  \if\relax\detokenize{#1}\relax\temp\else\let#1\temp\fi}

\getenv[\VERSION]{release_version}
```

と定義しておけば，文中に`\VERSION`と書くだけでタグ名（バージョン名）が出てきます．同期や先輩，先生に読んでもらうときに，タイトルに添えるなどしておくとよいでしょう．

```latex
\title{（卒論タイトル名）\\ \VERSION}
```

ちなみに`release_version`が定義されていないとき（ローカルでコンパイルするときなど）は，コンパイルエラーにはならず空白が出力されるようです．

### LaTeX文書で絵文字が使いたい
`.github/workflows/release.yml`の`Build PDF`のところでuseしている`KaiseiYokoyama/latex-build-langja@master`を`KaiseiYokoyama/latex-build-langja@emoji`に変えることで，[`BXcoloremoji`](https://github.com/zr-tex8r/BXcoloremoji)パッケージが使えます．

## ライセンス
MIT
ただし，予告なく変更される場合があります．
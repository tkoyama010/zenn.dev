---
title: "stlite と sphinx-revealjs でStremlitアプリをスライド化する"
emoji: "😸"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["streamlit", "sphinx", "revealjs"]
published: true
---

# stlite と sphinx-revealjs でStremlitアプリをスライド化する

## はじめに

この記事では、stlite と sphinx-revealjs を使って Streamlit アプリをスライド化する方法を紹介します。
stliteは、Streamlitアプリをサーバーなしで公開するためのツールです。
sphinx-revealjsは、SphinxでReveal.jsを使ってスライドを作成するためのツールです。
先日、sphinx-revealjs作者の[@attakei](https://github.com/attakei)さんが、stliteをsphinx上で表示する方法についてツイートをされていたので、それを参考にして、Streamlitアプリをスライド化する方法を試してみました。

https://twitter.com/attakei/status/1863087291612410362

上記を参考に、stliteとsphinx-revealjsを使って、Streamlitアプリをスライド化してみました。

## インストールと使い方

sphinx-revealjsとatsphinx-toyboxをインストールします。

```bash
$ python -m venv venv
$ ./venv/bin/activate
(venv) $ pip install sphinx-revealjs
(venv) $ pip install atsphinx-toybox
```

つぎに、sphinx-quickstartコマンドを使って、Sphinxプロジェクトを作成します。
sphinx-quickstartコマンドを実行すると、いくつかの質問が表示されますが、デフォルトのままEnterキーを押して進めていきます。
設定が必要な箇所は任意に設定してください。

```bash
(venv) $ sphinx-quickstart
```

次に、作成された `conf.py` に以下の設定を追加します。

```python
extensions = [
    'sphinx_revealjs',
    'atsphinx_toybox.stlite',
]
```

次に、`index.rst` に以下のように `stlite` ディレクティブが含まれる `sphinx-revealjs` のスライドを作成します。
`sphinx-revealjs` のスライドの作成方法については、[Sphinxでもプレゼンテーション](https://zenn.dev/attakei/books/presentation-sphinx/viewer/about-sphinx-revealjs)を参照してください。

```rst
Let's make slide using stlite and sphinx-revealjs!

.. stlite::
   :id: stlite-demo
   :requirements: matplotlib

   import streamlit as st
   import matplotlib.pyplot as plt
   import numpy as np

   size = st.slider("Sample size", 100, 1000)

   arr = np.random.normal(1, 1, size=size)
   fig, ax = plt.subplots()
   ax.hist(arr, bins=20)

   st.pyplot(fig)

```

実際にスライドを作成するために、以下のコマンドを実行します。


```bash
(venv) $ make revealjs
```

ローカルでスライドを確認するために、サーバーを起動します。

```bash
(venv) $ python -m http.server 11000 --directory _build/revealjs
```

ブラウザで `http://localhost:11000` にアクセスして、スライドを確認します。

今回な簡単な例以外にも[Sphinxでもプレゼンテーション](https://zenn.dev/attakei/books/presentation-sphinx/viewer/about-sphinx-revealjs)を参照することで、スライドのカスタマイズが可能です。

## さいごに

以下のOSS開発者の皆様に感謝いたします。
- sphinx-revealjsとatsphinx-toyboxの作者である[@attakei](https://github.com/attakei) さん
- stliteの作者である[@whitphx](https://github.com/whitphx) さん

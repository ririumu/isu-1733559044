## ブランチ

ブランチ練習を gh コマンドでしました。


### 手順

イシュー名に `feature $(date +%s)` を含めます。
そうすることを feature dates workflow と呼びます（仰々しい）。

1. イシューを作成  
まずイシューを作成します。  
`gh issue create --title "feature $(date +%s) description"`  
例: `gh issue create --title "feature $(date +%s) improve search"`

2. イシュー番号を確認  
つぎにイシューの番号を取得します。  
`gh issue list --state all`  
例:  
```
ID  TITLE
#5  feature 1733561849 improve search
```

3. ブランチを作成  
それから取得したイシュー番号でブランチを作成します。  
`gh issue develop ${イシュー番号}`  
例: `gh issue develop 5`  
生成されるブランチ名:  
`5-feature-1733561849-improve-search`

4. ブランチを確認  
`git branch -a`  
例:  
```
* main
  remotes/origin/5-feature-1733561849-improve-search
```

issue と branch が github 上に作成されました。


### 利点と事例

本ドキュメント https://github.com/ririumu/isu-1733559044/edit/main/about-gh-branch.md の初稿は
実際の feature dates workflow の適用例です。

まず、以下のコマンドで開始しました。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

作業の成果は [https://github.com/ririumu/isu-1733559044/pull/7](https://github.com/ririumu/isu-1733559044/pull/7) として main にマージされました。
作業のログは [https://github.com/ririumu/isu-1733559044/issues/6](https://github.com/ririumu/isu-1733559044/issues/6) より確認できます。

次に、作業に一意の名前 `feature 1733563997` があることの嬉しさを試してみました。ここでは `1733563997` が識別子になります。
例えば `git log --pretty=oneline | grep 1733563997` するなどで追跡してみました。

悪くはない感じでした。ただ結局 `$(date +%s)` のうれしさについてはあってもなくてもというところで、キラーな工夫であるかどうかは場合により、要するにどちらでも良かったです。


### 結論

https://github.com/ririumu/isu-1733559044/issues/6 を通じて gh コマンドの使い方を確認しました。

```
gh issue create --title "feature add metrics viewer"
gh issue develop 42
```


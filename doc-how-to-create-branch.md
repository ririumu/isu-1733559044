## `feature $(date +%s) topic description workflow` について

「ブランチは改修単位」を実現するコツを求めていて GitHub を練習していたところ、

* イシュー名を `feature $(date +%s) topic description` とする

のが良さそうでした。
これを `feature $(date +%s) topic description workflow` と呼ぶことにします。

個人的に使おうと思うので、備忘録として、使い方を記述します。

### 手順

1. イシューを作成  
まず改修単位に応じたイシューを作成します。  
`gh issue create --title "feature $(date +%s) Task description"`  
例: `gh issue create --title "feature $(date +%s) Add search functionality"`

2. イシュー番号を確認  
次に作成したイシューの番号を取得します。  
`gh issue list --state all`  
例:  
```
ID  TITLE
#5  feature 1733561849 Add search functionality
```

3. ブランチを作成  
それから取得したイシュー番号でブランチを作成します。  
`gh issue develop ${イシュー番号}`  
例: `gh issue develop 5`  
生成されるブランチ名:  
`5-feature-1733561849-add-search-functionality`

4. ブランチを確認  
`git branch -a`  
例:  
```
* main
  remotes/origin/5-feature-1733561849-add-search-functionality
```

### 利点

このワークフローの嬉しさは以下のとおりです。

- 命名規則: イシュー番号 + タスク名で直感的に分かる構造を維持。
- タイムスタンプ: ブランチ名の衝突を回避。
- Git コマンド中心: CLI 上で完結。

### 備考

本ドキュメントの手順では  `gh` コマンドを使用しましたが、これはブラウザで全ての操作を行うことができます。

その際は以下のコマンドなどで unixtime を取得します。

```
$ echo feature $(date +%s) topic description
feature 1733564884 topic description
```

### 事例

本ドキュメント https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md の初稿は、実際に本手順で作成しました。

作業開始のコマンドは以下でした。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

その成果物は、
https://github.com/ririumu/isu-1733559044/pull/7 として
最終的に `main` へマージされました。

作業のログが https://github.com/ririumu/isu-1733559044/issues/6 で確認できます。

`feature $(date +%s) topic description workflow` に従うことで、
このように、あとで作業の追跡もしやすくなります。

なお `isu-1733559044` のコミットログを眺めれば分かるとおり、改稿は main で行なっています。 main への直接 push を許すかどうかはチームの方針によりますが、今回は自明に良いことはレビューが必要ない、またコミットログは dirty であっても構わないと、チーム（＝私）が考えたことによって main への直接 push は allow となっています。

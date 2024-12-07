## feature $(date +%s) topic description workflow

「ブランチは改修単位」を実現するワークフローとして、イシュー名を `feature $(date +%s) topic description` にすることがちょうど良さそうです。

その手順を記述します。

## 手順

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

## 利点

このワークフローの嬉しさは以下のとおりです。

- 命名規則: イシュー番号 + タスク名で直感的に分かる構造を維持。
- タイムスタンプ: ブランチ名の衝突を回避。
- Git コマンド中心: CLI 上で完結。

## 備考

本ドキュメントの手順では  `gh` コマンドを使用しましたが、これはブラウザで全ての操作を行うことができます。

その際は `echo` コマンドで unixtime を取得することが役立つかもしれません。

```
$ echo feature $(date +%s) topic description
feature 1733564884 topic description
```

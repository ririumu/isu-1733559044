## ブランチ

作業単位に対するブランチの練習を gh コマンドでしました。


### 手順

せっかく cli を使うのでイシュー名に `feature $(date +%s)` を含めてみます。
そうすることを feature dates workflow と呼んでみます。

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


### 利点

このワークフローの嬉しさは以下のとおりでした。

- 命名規則（イシュー番号 + タスク名）が直感的な理解を促進する。
- タイムスタンプ: UNIXタイムスタンプに識別子としての多面的な利用性がある。

これはブラウザで全ての操作を行うことができるので、作業は CLI メインで作業してもよいし GUI (ブラウザ) メインで作業してもよいです。


### 事例

本ドキュメント [https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md](https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md) の初稿は、実際に本手順で作成しました。
作業開始のコマンドは以下でした。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

作業の成果物は [https://github.com/ririumu/isu-1733559044/pull/7](https://github.com/ririumu/isu-1733559044/pull/7) として main にマージされました。
作業のログは [https://github.com/ririumu/isu-1733559044/issues/6](https://github.com/ririumu/isu-1733559044/issues/6) から確認できます。

これにより、作業に一意の名前 `feature 1733563997` がつきました。
unixtime `1733563997` を識別子として定めて `git log --pretty=oneline | grep 1733563997` するなどで追跡してみました。

以上の通り feature dates workflow は幾らかのメリットがありました、ですが、マイナーな workflow はそれだけで良くないです。

### 結論

gh で新しくブランチを切る際に issue を経由する場合はこうすると良いです。

```
gh issue create --title "feature add metrics viewer"
gh issue develop 42
```

普通ですね。
よって結局 https://github.com/ririumu/isu-1733559044/issues/6 はコマンド練習の記録なのでした。

おしまい

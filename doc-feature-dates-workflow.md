## feature dates workflow について

「ブランチは改修単位」を実現するコツを求めていて GitHub を練習していたところ、
イシュー名を `feature $(date +%s)` を含めるとよさそうだと思いました。

これを feature dates workflow と呼ぶことにします。

個人的に使おうと思うので、備忘録としてドキュメントを作成します。


### 手順

1. イシューを作成  
まず改修単位に応じたイシューを作成します。  
`gh issue create --title "feature $(date +%s) description"`  
例: `gh issue create --title "feature $(date +%s) improve search"`

2. イシュー番号を確認  
次に作成したイシューの番号を取得します。  
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


### メリット

feature dates workflow のメリットは以下のとおりです。

- 命名規則（イシュー番号 + タスク名）が直感的な理解を促進する。
- タイムスタンプ: UNIXタイムスタンプに識別子としての多面的な利用性がある。
- gh は必須ではなくブラウザで事足ります `date +%s` だけ覚えていれば。


### 事例

本ドキュメントの初稿に feature dates workflow を採用しました。
作業開始のコマンドは以下でした。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

作業の成果物は [https://github.com/ririumu/isu-1733559044/pull/7](https://github.com/ririumu/isu-1733559044/pull/7) として main にマージされました。
作業のログは [https://github.com/ririumu/isu-1733559044/issues/6](https://github.com/ririumu/isu-1733559044/issues/6) から確認できます。

feature dates workflow によって、識別子である `feature 1733563997` がつき、これは些細ながら、追跡性をアップさせました。


### まとめ

イシュー名を `feature $(date +%s) descripton` とすると、良さそうでした。

ただし feature dates workflow はあまりにもマイナーなので（だってさっき考えたので）、通常の フィーチャーブランチ でも良いかと思われます。

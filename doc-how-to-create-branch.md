## `feature $(date +%s) topic description workflow` について

「ブランチは改修単位」を実現するコツを求めていて GitHub を練習していたところ
イシュー名を `feature $(date +%s) topic description` とする
のが良さそうでした。

これを `feature $(date +%s) topic description workflow` と呼ぶことにします。

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


### 利点

このワークフローの嬉しさは以下のとおりです。

- 命名規則（イシュー番号 + タスク名）が直感的な理解を促進する。
- タイムスタンプ: UNIXタイムスタンプに識別子としての多面的な利用性がある。


### 備考

本ドキュメントの手順では  `gh` コマンドを使用しましたが、これはブラウザで全ての操作を行うことができます。
その際は以下コマンドで unixtime を取得します。

```
$ echo feature $(date +%s) topic description
feature 1733564884 topic description
```

つまり、作業は CLI メインで作業してもよいし、GUI (ブラウザ) メインで作業してもよいです。

### 事例

本ドキュメント [https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md](https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md) の初稿は、実際に本手順で作成しました。

作業開始のコマンドは以下でした。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

作業の成果物は [https://github.com/ririumu/isu-1733559044/pull/7](https://github.com/ririumu/isu-1733559044/pull/7) として main にマージされました。
作業のログは [https://github.com/ririumu/isu-1733559044/issues/6](https://github.com/ririumu/isu-1733559044/issues/6) から確認できます。

本稿作成にあたり `feature $(date +%s) topic description workflow` に従うことで、その作業に一意の名前 `feature 1733563997` がつきました。
かつこの時 unixtime `1733563997` がイミュータブルな識別子として定まったので `git log --pretty=oneline | grep 1733563997` するなど、以降の作業の追跡がしやすそうです。


### 議論

なお `isu-1733559044` のコミットログを眺めれば分かるとおり、改稿はほとんど main ブランチ上で行なっています。 

**「main への直接 push を許すべきか？」**

これはチームの方針によりますが今回は

* 自明に良いことはレビューが必要ない
* またコミットログは dirty であっても構わない (=~ [rebase しない派](https://x.com/tadsan/status/1863567234947571934))

とチーム（＝私）が考えたことによって、 main push を allow しています。

一方で feature branch を消さないでおいて引き続き作業して main merge させることにも一定のメリットがあります。

例えば `git log --pretty=oneline | grep 1733563997` では main push のコミットを特定することができないので、若干追跡性が落ちます。
feature branch で更新を行うことは main のコミットログに関心あるチームメイトにとって、メリットがあります。

```
$ git log --pretty=oneline | grep 1733563997
acfc0fc70e64c1db119baae15b986d6caf35bc52 Merge pull request #9 from ririumu/6-feature-1733563997-write-doc
6c6b4bba74c86475eefb9abfcca8beaa49307dc0 Merge pull request #7 from ririumu/6-feature-1733563997-write-doc
```

個人としてのスピードを犠牲にしてでも、チームとしての生産性を向上させたい場合は、おそらくそうするべきです。
この変更も feature branch で作業をしています。ただやっぱり、手数が多くなってしまうわけなので、どちらがいいのでしょうね・・・。

まとめると以下の3択です。

1. セルフマージが多い場合は main push をしていいものとする
2. main の品質のため main push は disallow であり doc update や fix typo レベルであっても feature branch で作業するものとする
3. 上記2つの中間で main push と feature merge の双方を適宜ベストだと思うように運用する

今回は3を選んでいます（というか結果的にそうなった）。


### まとめ

イシュー名を `feature $(date +%s) topic description` とすると便利そうです。


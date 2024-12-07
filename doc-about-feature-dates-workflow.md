## feature dates workflow 

`feature $(date +%s)` ってどうかなあと思っていたのですが、、、現時点の所感は普通です。

まず「ブランチは改修単位」を実現するコツを個人的に求めており GitHub を練習していたところ
イシュー名を `feature $(date +%s) description` とするのが良さそうだと感じる瞬間がありました。
これを feature dates workflow と呼ぶことにして、備忘録と練習をしてみました。

結論としてはそこまで仰々しいものではなかったので読まなくて良いです。


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

なお、本稿の手順では  `gh` コマンドを使用しましたが、これはブラウザで全ての操作を行うことができます。
つまり、作業は CLI メインで作業してもよいし、GUI (ブラウザ) メインで作業してもよいです。


### 議論 : main push について

しかし `isu-1733559044` のコミットログを眺めれば分かるとおり、改稿はほとんど main ブランチ上で行なっています。 

**「main への直接 push を許すべきか？」**

これはチームの方針によりますが今回は

* 自明に良いことはレビューが必要ない
* またコミットログは dirty であっても構わない (=~ [rebase しない派](https://x.com/tadsan/status/1863567234947571934))

とチーム（＝私）が考えたことによって、 main push を allow しています。

一方で feature branch 作業は feature dates workflow にとって一定のメリットがありそうでした。
例えば `git log --pretty=oneline | grep 1733563997` では main push のコミットを特定することができません。
しかし feature branch 作業については `grep 1733563997` で見つけることができます。

```
$ git log --pretty=oneline | grep 1733563997
acfc0fc70e64c1db119baae15b986d6caf35bc52 Merge pull request #9 from ririumu/6-feature-1733563997-write-doc
6c6b4bba74c86475eefb9abfcca8beaa49307dc0 Merge pull request #7 from ririumu/6-feature-1733563997-write-doc
```

このため以下の3択が考えられます。

1. セルフマージが多い場合は main push をしていいものとする
2. main の品質のため main push は disallow であり doc update や fix typo レベルであっても feature branch で作業するものとする
3. 上記2つの中間で main push と feature merge の双方を適宜ベストだと思うように運用する

今回は3を選んでいます（というか結果的にそうなった）。


### 事例

本ドキュメント [https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md](https://github.com/ririumu/isu-1733559044/blob/main/doc-how-to-create-branch.md) の初稿は、実際に本手順で作成しました。
作業開始のコマンドは以下でした。

```
gh issue create --title "feature $(date +%s) write doc"
gh issue develop 6
```

作業の成果物は [https://github.com/ririumu/isu-1733559044/pull/7](https://github.com/ririumu/isu-1733559044/pull/7) として main にマージされました。
作業のログは [https://github.com/ririumu/isu-1733559044/issues/6](https://github.com/ririumu/isu-1733559044/issues/6) から確認できます。

本稿作成にあたり feature dates workflow に従うことで、その作業に一意の名前 `feature 1733563997` がつきました。
unixtime `1733563997` を識別子として定めて `git log --pretty=oneline | grep 1733563997` するなどで追跡してみました。


### まとめ

GitHub イシュー名に `feature $(date +%s)` 含めることを feature dates workflow と呼ぶことにして、それについて、議論と作業をしました。
とはいえマイナーな workflow はそれだけで良くないですね…。「GitHub を用いたチーム開発ではブランチを改修の単位とするとよい」で事足りる感じがしました。
（そもそもそんなに unixtime が便利なら github にすでに統合されているはずだという気もしてきた）

「普通の機能ブランチ」に寄せたコマンドは以下です。

```
gh issue create --title "feature add metrics viewer"
gh issue develop 42
```

普通ですね。
よって結局 https://github.com/ririumu/isu-1733559044/issues/6 はコマンドの練習でした。

おしまい

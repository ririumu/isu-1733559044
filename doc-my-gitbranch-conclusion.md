## ブランチとの向き合った結論

色々頑張ったんですけどねえ…。

### ブランチとの向き合った結論 => 素直に github を使おう

結局、issue を作って、それからその issue に紐づく branch を作り、それをチェックアウトして、開発して、プルリクを作る、これでよかったです。

```
git pull                              # おまじない的にやっていますが意味はないかもしれません
gh issue create --title "improve xyz" # イシューを作ってくれます (ブラウザ操作でも良いです)
gh issue develop 42                   # ブランチを作ってくれます (ブラウザ操作でも良いです)
git pull                              # おまじない的にやっていますが意味はないかもしれません

git remote -a                         # ブランチの名前を確認できます
git checkout <作成されたブランチの名前>   # 作業がうっかり main に入らないために忘れずに
code .                                # vscode を使うことを仮定しています

git branch                            # 作業ブランチが適切か確認します
git commit                            # 作業結果をコミットします
git push                              # 作業結果をリモートリポジトリに共有します

gh pr create                          # プルリクを作ってくれます (ブラウザ操作でも良いです)
```

### やっておくべき設定 => main push は禁止する

`main` は常に一定品質を保たれていることが期待されています。main push は禁止するべきです。人はミスを犯すからです。

<img width="909" alt="image" src="https://github.com/user-attachments/assets/30469d8b-ca92-4d4e-a25f-5c8bfb678c93">

### 結論

色々試してみましたが、素直に github のやり方に従うのが良かったです。 

git だけでなんとかするのしんどいです（少なくとも自分は）。「マイナな WF を考えてしまった…」と後悔していた [feature dates workflow](https://github.com/ririumu/isu-1733559044/blob/main/markdown/feature-dates-workflow.md) ですが、しかし普通であるが故に、まだかなりマシで、それに、たとえ [このように](https://github.com/ririumu/isu-1733559044/issues?q=is%3Aissue+is%3Aclosed) issue が混沌としていても dates workflow で建てられた issue は命名規則に従っているため、比較的まともな issue っぽい空気が出ていました。

以下は feature dates workflow で作ったものです。ツリーが綺麗です。

```
*   009cd09 - (HEAD -> main, origin/main, origin/HEAD) Merge pull request #25 from ririumu/23-feature-1733593706-remove-unusable-method-from-doc (9 minutes ago) <ririumu>
|\
| * 4bd72b0 - doc update (9 minutes ago) <ririumu>
|/
*
```

おそらく普通のフィーチャーブランチも同じ綺麗さになることでしょう。
なので普通を確認するだけでした。

こちらからは以上です。


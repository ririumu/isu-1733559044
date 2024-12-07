## ブランチとの向き合い

なんか色々言いたいことあるけど結局これがすべてなあ。

```
*   009cd09 - (HEAD -> main, origin/main, origin/HEAD) Merge pull request #25 from ririumu/23-feature-1733593706-remove-unusable-method-from-doc (9 minutes ago) <ririumu>
|\
| * 4bd72b0 - doc update (9 minutes ago) <ririumu>
|/
*
```

つまり issue を作って、それからその issue に紐づく branch を作り、それをチェックアウトするという流れは、無難だし、イケています。

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

vscode ではプルリクのマージまで一気通貫にやってくれる印象だったので `code .` 以降は不要である説があります。

色々試してみましたが、素直に github のやり方に従うのが良かったです。 git だけでなんとかするのしんどいです（少なくとも自分は）。「マイナな WF を考えてしまった…」と後悔していた [feature dates workflow](https://github.com/ririumu/isu-1733559044/blob/8b1d503f1c1976e1318a3b09204b1bd48387cf26/doc-about-feature-dates-workflow.md?plain=1) ですが、しかし普通であるが故に、まだかなりマシでした。

### 悲劇の未然防止 : main push の禁止

`main` は常に一定品質を保たれていることが期待されており、おそらく main push は禁止するべきです

<img width="909" alt="image" src="https://github.com/user-attachments/assets/30469d8b-ca92-4d4e-a25f-5c8bfb678c93">

### 結論

素直に github のやり方に従うのが良いです。 main push は禁止で設定するのが良いです。

[feature dates workflow](https://github.com/ririumu/isu-1733559044/blob/8b1d503f1c1976e1318a3b09204b1bd48387cf26/doc-about-feature-dates-workflow.md?plain=1) は悪くなかったです。たとえ issue が [このように](https://github.com/ririumu/isu-1733559044/issues?q=is%3Aissue+is%3Aclosed) 混沌としていても、それが命名規則に従っているという理由により、まともな issue であるということがハッキリします。

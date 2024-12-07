## ブランチ

ブランチ操作を伴う開発を行うときの gh と git コマンドのサンプルです。 gh は cli で github を操作できるツールですが必須ではありません。

### パターン1 : issue を使う

以下は issue を作って、それからその issue に紐づく branch を作る場合の流れです。

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

なお vscode ではプルリクのマージまで一気通貫にやってくれる印象だったので `code .` 以降は不要である説があります。

### パターン2 : issue は使わない pr だけ使う

```
git checkout -b foo remotes/origin/main  # 作業ブランチを foo とします
git branch                               # foo になっていることを確認します

# そのブランチに対応する作業をします
echo foo > foo.txt
git add foo.txt
git commit -m foo

# 変更を反映します origin (プッシュ先) と foo (ローカルブランチ) を指定することがベターです
git branch origin foo
```


## ブランチ

ブランチ操作を伴う開発を行うときの gh と git コマンドのサンプルです。 
gh は cli で github を操作できるツールですが必須ではありません。
そもそも github のブラウザ操作が前提であることが多いですので。

また本稿では「そもそも Git や GitHub にどう向き合うべきか」という観点も提供できれば、嬉しいです。
それを意識して書いています。

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

ローカルリポジトリで任意の名前をブランチ名にするやり方です。こちらの方が本来の Git 的であり GitHub への依存が少ないです。

```
git checkout -b foo remotes/origin/main  # 作業ブランチを foo とします
git branch                               # foo になっていることを確認します
code .                                   # そのブランチに対応する作業をします
git push origin foo                      # origin (プッシュ先) と foo (ローカルブランチ) を指定する必要があります
```

注意しなければならないのは `git push origin foo` は結構大事なコマンドだということです。

```
$ git push origin foo
Enumerating objects: 7, done.
Counting objects: 100% (7/7), done.
Delta compression using up to 8 threads
Compressing objects: 100% (3/3), done.
Writing objects: 100% (4/4), 364 bytes | 364.00 KiB/s, done.
Total 4 (delta 2), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (2/2), completed with 2 local objects.
To https://github.com/ririumu/isu-1733559044
   a2baf19..01c0f54  foo -> foo
```

なぜならばローカルリポジトリの `foo` ブランチの成果は、
確実にリモートリポジトリの `origin/foo` に届けなくてはならないからです（そうでなくてはPRの意味が崩壊します）。
しかしながら vscode で下手に push してしまうとどうやら `foo` の成果物が `origin/main` に行ってしまうので事故ることを確認しました。
これ言い換えると `origin/foo` に push すべきものが `origin/main` に push されるという悲劇が発生するということです。

同様にブラウザで操作した成果をローカル PC に持ってくる時はこれも逆もまた然りということで

```
git pull origin foo
```

を使うべきです。これもかなり大事で、

```
git pull origin foo
code .
gh pr create
```

というコマンドの打ち方の流れがあります。

```
$ gh pr create
a pull request for branch "foo" into branch "main" already exists:
https://github.com/ririumu/isu-1733559044/pull/14
```

「もうあるよそれ」と言われても URL を教えてくれるのでありがたいです。

```
git pull origin foo && git push origin foo
```

上記のコマンドは端的に行って「処方箋」となります。
なぜなら一人で ISUCON するときは `count` という雑なコミットメッセージしますよね。

でチーム開発だからといって教条的に pr とか review すると明らかに速度が落ちます。
こころらへんのバランスの取り所は本当に、難しいですね。


### 悲劇を防ぐための main push の禁止

さて、ミスをするのは99.9%人間です。特に、体調が良くないなど、疲れていると人間はミスをします。
そして `main` は常に一定品質を保たれていることが期待されます。

<img width="909" alt="image" src="https://github.com/user-attachments/assets/30469d8b-ca92-4d4e-a25f-5c8bfb678c93">

だからやっぱり、悲劇をを防止するに、 main push を禁止するよう github を設定するべきと言えるのでしょう。

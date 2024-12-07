## ブランチとの向き合い

### はじめに

git はそもそもブラウザは関係ないツールでした。
そもそも git は linux のために linus が作ったもので github とは関係がなかったのです、歴史上。
他方で github のブラウザ操作を基本としそれで完結させようとするツールです。

そこで本稿では「そもそも Git や GitHub にどう向き合うべきか」という観点も提供できれば、嬉しいです。

その辺りも踏まえて意識して、しかしながらできるだけ簡単に、しかし私のお気持ちも含めて本稿を書いています。
ブランチ操作を伴う開発を行うときの、CLI コマンドのサンプルを、できるだけわかりやすく書こうとしています。

この記事が何かのヒントになると嬉しいです。

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
元々 git は linus が linux を作るために作ったツールで、当時はメーリングリストでのやり取りが主だったのです。

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
しかしながら vscode で下手に push してしまうとどうやら `foo` の成果物が `origin/main` に行ってしまうので事故ることを確認しました（原因を deep dive してませんがとにかく起きました）。
これ、言い換えると main push を allow していると「`origin/foo` に push すべきものが `origin/main` に push されてしまった」という大悲劇が発生しうる、ということです。

同様にブラウザで操作した成果をローカル PC に持ってくる時はこれも逆もまた然りということで `git pull origin foo` もかなり大事です。
すなわち、以下のようなコマンドの打ち方の手順があるということです。

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
しかも `origin/foo` にコミットは蓄積されておりますので。

それから。下記コマンドは、上記議論を踏まえるとかなり「処方箋」となります。

```
git pull origin foo && git push origin foo
```

なぜなら一人で ISUCON するときは `count` という雑なコミットメッセージしますよね。
でチーム開発だからといって教条的に pr とか review すると明らかに速度が落ちます。

こころらへんのバランスの取り所は本当に、難しいですね。


### 悲劇の未然防止 : main push の禁止

さて、ミスをするのは99.9%人間です。特に、体調が良くないなど、疲れていると人間はミスをします。
そして `main` は常に一定品質を保たれていることが期待されます。

<img width="909" alt="image" src="https://github.com/user-attachments/assets/30469d8b-ca92-4d4e-a25f-5c8bfb678c93">

だからやっぱり、悲劇をを防止するに、 main push を禁止するよう github を設定するべきと言えるのでしょう。
私たちは常に元気溌剌ではないのです。

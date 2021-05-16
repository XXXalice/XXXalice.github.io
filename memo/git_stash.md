## 間違って別のブランチで実装を始めちゃった時にgit stashで別ブランチに編集中のソースを移動する

* git
* Git - Stashingの手抜き翻訳かつ、勝手に構成や説明の流れを変えたものです。


間違ってmasterブランチなどで実装を始めたのに途中で気づいて、開発用のdevelopmentブランチにその書きかけのソースをコミットすることなく持って行きたいときは、git stashを使う。

書きかけだとgit statusとしたときに色々でるはず。

```
$ git status
# On branch master
# Changes to be committed:
#   (use "git reset HEAD <file>..." to unstage)
#
#      modified:   index.html
#
# Changes not staged for commit:
#   (use "git add <file>..." to update what will be committed)
#
#      modified:   lib/simplegit.rb
#
```

書きかけのソースをコミットせずに、以下のコマンドを入力する。

```
$ git stash
```

そうすると、一時フォルダ的なstashリストに書きかけだったソースが退避されて登録されるので、git statusも以下のようになる。

```
$ git status
# On branch master
nothing to commit (working directory clean)
```

今退避されているソースのリストは、git stash listコマンドで確認できる。


```
$ git stash list
stash@{0}: WIP on master: 049d078 added the index file
stash@{1}: WIP on master: c264051... Revert "added file_size"
stash@{2}: WIP on master: 21d80a5... added number to log
```

この状態でコードを持って行きたい適当なブランチに切り替える。
ブランチの切り替え方はgitのローカルbranchを作成し、リモートにpushする - sessanの日記参照。

ブランチを切り替えた後、最後にstashしたものを反映したい場合は、

```
$ git stash apply 
```

とする。

特定のstashリストを指定して反映したい場合は、

```
$ git stash apply stash@{2}
```

のように、stash listで表示されていた名前を指定してapplyする。


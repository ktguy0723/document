# git運用方針

基本コマンドおよび推奨方式の整理を行う。

##  概要

- 運用方針



## 詳細

### 1. 環境構築フェーズ

プロジェクト参画時、初回のみ実行する。初回以外の場合は飛ばしてください。

**前提条件**

- Windows(10)想定
- Git BashでのCLIでの操作を想定



#### 1.1. インストール

1. Git for windowsからインストーラをインストールし、インストールする。
   設定はデフォルト値でOK。

2. [Windowsボタン]を押下後、`git bash`と入力し、Git Bashを開く。
   以降は、Git Bashで操作する。

   

3. 正しくインストールされているか確認する。以下のコマンドを実行する。

   ```bash
   $ git version
   ```

   バージョン情報が表示されればOK。表示されなかった場合は知見のある方に助けを求めて。



#### 1.2. 初期設定

1. ユーザー情報の登録
   ユーザ名とメールアドレスを登録する。

   ```bash
   $ git config --global user.name "あなたの名前"
   $ git config --global user.email hoge@example.com
   ```

   登録されていることを確認する。

   ```bash
   $ git config user.name
   $ git config user.email
   
   # 以下で一覧の確認も可能
   $ git config -l
   ```

   ※ 設定した値は、`~/.gitconfig`に記述してある。
   
2. Git for Windowsにおける改行コードの設定
   デフォルト設定だと、checkout（pull）や、commit時に勝手に改行コードを変換してしまうので自動変換されないよう修正。

   ```bash
   $ git config --global core.autocrlf false
   $ git config core.autocrlf
   false
   ```

   ※ 参考：https://qiita.com/uggds/items/00a1974ec4f115616580

   

3. プロキシの設定
   プロキシ下で使用する場合は設定。

   ```bash
   $ git config --global http.proxy http://[proxy]:[port]
   $ git config http.proxy
   ```

   

### 2. 開発フェーズ（初回）

1. ローカルリポジトリを新規作成する場合

   ```bash
   $ git init
   ```

2. リモートリポジトリからClone（コピー）して環境構築する場合

   ```bash
   # master(main)ブランチのClone
   $ git clone <リポジトリ名>
   
   # ブランチを指定してClone
   $ git clone -b ブランチ名 https://リポジトリのアドレス
   ```



### 3. 開発フェーズ（通常）

1. 新規作成、更新

   ```bash
   #ステージング（ステージにインデックスを追加）
   $ git add <ファイル名 or ディレクトリ名>
   $ git add . #【別法】ワークツリー（作業ディレクトリ）に含まれるすべてのファイルに対して実行
   
   #確認（ステージング）
   $ git status #変更状況の確認
   $ git diff	#ワークツリーとステージ（インデックス）の差分比較
   
   #コミット（スナップショットを記録）
   $ git commit -m "<メッセージ>"
   $ git commit -v #【別法】viが立ち上がり、変更差分を確認しながらコミットメッセージを登録
   
   #確認（コミット）
   $ git status
   $ git diff --staged #ステージとローカルリポジトリの差分比較
   $ git log #変更履歴を確認する
   $ git log --oneline #【別法】1行で履歴を表示
   $ git log -p <ファイル名> #【別法】指定ファイルの変更差分を表示
   
   #プッシュ
   $ git remote # リモート名を表示
   origin
   $ git remote add origin <リモートURL> #【opt】リモートリポジトリが登録されていない場合
   $ git push <リモート名=origin> <ブランチ名>
   $ git push -u origin master #【別法】次回以降 origin masterにプッシュするときgit pushだけでOK
   ```

2. ファイル削除

   ```bash
   $ git rm <ファイル名>
   $ git rm -r <ディレクトリ名>
   $ git rm --cached <ファイル名> # ファイルを残してリポジトリには残したくない場合
   
   $ git commit -m "ファイル削除"
   ```

3. ファイルを移動

   ```bash
   $ git mv <旧ファイル> <新ファイル>
   $ git commit -m "ファイル移動"
   
   # 下記と同じ
   $ mv <旧ファイル> <新ファイル>
   $ git rm <旧ファイル>
   $ git add <新ファイル>
   ```

   

4. 変更の取り消し

   1. **stageする前**に変更を取り消す場合

      ```bash
      $ git checkout -- <ファイル名>
      $ git checkout -- <ディレクトリ名>
      $ git checkout -- . #全変更の取り消し
      ```

      

   2. **stageした変更**を取り消す場合
      `HEAD`は最新のコミットを意味する。最新のコミットで上書きするイメージ

      ```bash
      $ git reset HEAD <ファイル名>
      $ git reset HEAD <ディレクトリ名>
      $ git reset HEAD .
      
      $ git checkout -- . #ワークツリーの変更は自動で行われないので、コマンドを実行する必要がある
      ```

      

   3. 間違えてcommitした場合
      **リモートリポジトリにpushしたものを`amend`でやり直すことは絶対ダメ。新しいコミットでやり直そう**

      ```bash
      $ git add . #ステージングした後に実行
      $ git commit --amend
      ```

5. オプション

   ```bash
   # エイリアスを設定
   $ git config --global alias.st status #git status => git stと設定
   $ git st
   ```




**備考：gitconfigについて**

gitconfigの設定には３つの段階がある。有効優先度は①、②、③の順である。

① local: .git/config に設定
② global : ~/.gitconifgに設定
③ system : gitインストールディレクトリのgitconfigファイルに設定

それぞれ、下記のように設定・参照が可能

```bash
# local
$ git config --local user.name "My Name"
$ git config --local --list

# global
$ git config --global user.name "My Name"
$ git config --global --list

# system
$ git config --system user.name "My Name"
$ git config --system --list
```

参考：https://qiita.com/shionit/items/fb4a1a30538f8d335b35





## 4. リモートリポジトリ方針

```bash
#リモートリポジトリを表示
$ git remote -v 

#リモートリポジトリを追加登録する
$ git remote add <リモート名> <リモートURL>

#リモートから取得(fetch -> merge)
$ git fetch <リモート名>
$ git fetch origin #originからとってくる
$ git branch -a # branchの状況を確認
$ git merge origin/master #origin/masterをワークツリーとマージ

#リモートから取得(pull)
$ git pull origin master
# 以下と同じ
$ git fetch origin
$ git merge origin/master

#リモート名の変更
$ git remote rename <旧リモート名> <新リモート名>

#リモートの削除
$git remote rm <リモート名>
```



## 5. ブランチ方針

1. ブランチ

   - ブランチはコミットファイルを指すポインタ
   - HEADは自分の作業しているブランチを指すポインタ

   ```bash
   # ブランチの新規作成
   $ git branch <ブランチ名>
   
   # ブランチの一覧を表示
   $ git branch
   $ git branch -a #リモートブランチも含めて確認
   
   # ブランチを切り替える
   $ git checkout <既存ブランチ>
   
   # ブランチ名の変更
   $ git branch -m <ブランチ名>
   
   # ブランチの削除
   $ git branch -d <ブランチ名>
   $ git branch -D <ブランチ名> # マージされていない変更があっても削除
   ```

   

2. マージ

   ```bash
   $ git merge <ブランチ名>
   $ git merge <リモート名/ブランチ名>
   $ git merge origin/master
   ```

   - Fast Forward : ブランチが枝分かれしていない場合、ポインタを前に進めるだけ
   - Auto Merge : ブランチが枝分かれしている場合、新しいマージコミットを作る
   - Conflict :Auto MergeがConflictによって失敗した場合、ファイルを修正して stage => commit を実行

   

   conflictが発生したら、conflict個所を修正して下記を実行

   ```bash
   $ git add .
   $ git commit -m "conflictを解消"
   ```

   

   conflict対策

   - 複数人で同じファイルを変更しない
   - pullやmergeをする際は変更中のファイルをなくす（commit, stashしておく）
   - pullする際は、pullするブランチとpullされるブランチが同じか確認する

   



## 6. 開発手法整理

1. プルリクエスト

   自分の変更をリポジトリに取り込んでもらうよう依頼する機能

   1. (レビューア ローカル) masterブランチをプル
   2. (レビューア ローカル) ブランチを作成
   3. (レビューア ローカル) ファイルを更新
   4. (レビューア ローカル) 変更をコミット
   5. (レビューア ローカル) GitHubにプッシュ
   6. (レビューア) プルリクエストを送る
   7. (レビューイ) コードレビュー
   8. (レビューイ) プルリクエストをマージ
   9. (レビューイ) ブランチを削除



## 7. 【応用】リベース、タグ付け、スタッシュ

#### リベース: 履歴を整える

```bash
$ git rebase <ブランチ名>
```

- リモートリポジトリにプッシュしたコミットをリベースすると情報矛盾が発生しエラーとなる
- `git push -f` は強制的にPushできるが、履歴破壊となるため絶対に使わない
- 作業の履歴を残したい場合は`merge`。そうでない場合は`rebase`を使おう。
- conflictの解消が面倒なのでconflictが発生しそうならmergeを使おう。



#### タグ：コミットを参照しやすくするために名前を付ける

- 一般的にリリースポイントに使う

```bash
#タグを表示する
$ git tag
$ git tag -l "202202" #パターンを指定してタグを絞り込む
$ git show <タグ名> #タグ情報の詳細を表示する

#タグをつける
$ git tab -a <タグ名> -m "<メッセージ>" #注釈付きタグ(タグ名、コメント、署名)
$ git tag <タグ名> #軽量版タグ（タグ名だけ）
$ git tag <タグ名> <コミットID> #昔のコミットにもタグをつけられる

#リモートリポジトリに送信する
$ git push <リモート名> <タグ名> #タグを指定して送信
$ git push origin 20220202 
$ git push origin --tags #すべてのタグの一斉送信
```



#### スタッシュ：作業を一時避難する

```bash
# stashに変更を一時退避する
$ git stash
$ git stash save #上記と同義

# 退避した作業の一覧を確認
$ git stash list

# 退避した作業を復元する
$ git stash apply --index # 直前の作業を復元する
$ git stash apply [スタッシュ名] # 特定の作業を復元する。スタッシュ名はgit stash list で確認可能
$ git stash apply stash@{1}

# 退避した作業を削除する
$ git stash drop #直前の作業を削除
$ git stash drop <スタッシュ名> #特定の作業を削除
$ git stash drop stash@{1}
$ git stash clear #全作業を削除
```

## 8. Gitセーフティーネット

### 8.1. 間違って編集してしまった。`add`はしていないが、直前のコミットに戻したい。
```bash
$ git checkout -- <ファイル名>
$ git checkout -- <ディレクトリ名>
$ git checkout -- . #全変更の取り消し
```
### 8.2. 間違って`add`してしまった。`commit`はしていないが、直前のコミットに戻したい。
`HEAD`は最新のコミットを意味する。最新のコミットで上書きするイメージ
```bash
$ git reset HEAD <ファイル名>
$ git reset HEAD <ディレクトリ名>
$ git reset HEAD .

$ git checkout -- . #ワークツリーの変更は自動で行われないので、コマンドを実行する必要がある
```

### 8.3. 間違って`commit`してしまった。`push`はしていないが、直前のコミットに戻したい。
```bash
# masterのHEAD、インデックス、ワーキングツリー全てを１つまえに戻す
$ git reset --hard HEAD~
```

### 8.4. コミットメッセージを修正したい。      
```bash
$ git add . #ステージングした後に実行
$ git commit --amend -m "コミットメッセージ"
```

### 8.5. 作業中に別作業の依頼が来てしまった。退避して別作業を実施する。
```bash
$ git stash -u # 作業中の内容を根こそぎ退避
$ git checkout -b other-branch # 別のブランチを切って別の作業をこなす
# 作業
$ git add <必要なファイル>
$ git commit -m "コミットメッセージ"
$ git checkout origin-branch # 元のブランチに戻る
$ git stash pop # 退避していた作業内容を取り出す
```

### 8.6. 特定のコミットまで戻して、作業をやり直したい
```bash
$ git checkout <コミットID>
$ git checkout -b new-branch # 別のブランチを作成
```

> 参考になるQiita
> https://qiita.com/muran001/items/dea2bbbaea1260098051

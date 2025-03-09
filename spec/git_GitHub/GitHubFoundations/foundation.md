## 1. GitとGithub　(２２％)
### Git and GitHub Basic
- バージョンコントロールシステム
  - Centralized VCS
  - Distributed VCS(Git)
- Gitの基礎
  - コミットで作成されるスナップショット
  - コミットからコミットへのリンクによるグラフ形成
  - ブランチ
- GitHub Flow
  - プルリクエストを利用するフロー

### GitHub Entities
- GitHub accounts
  - Personal accounts
  - Organization accounts
  - Enterprise accounts
- Plan
  - Free: 無料、Personal、Organization
  - Pro: 有料、Personal
  - Team: 有料、Organization
  - Enterprise: 有料、Enterprise
- プロフィール機能
  - 実績
  - リポジトリ、スター
  - ピン留めされたリポジトリ
  - bio
  - プロフィールREADME
    - 自分とユーザ名と同じリポジトリを作成し、README.mdファイルを作成すると作ることができる

### GitHub Markdown
- Slash command
  - '/'を打つと補足を出してくれる

### GitHub にアクセスする方法
- ブラウザ
- GitHub Desktop
- GitHub Mobile 

## 2.GitHubリポジトリの操作　(８％)
- リポジトリで推奨されるファイル
  - README.md
  - FUNDING.yml
  - CODEOWNERS.md
  - CITATION.cff
  - SECURITY.md: エラー発生時にどこに連絡すべきか
  - CODE_OF_CONDUCT.md
  - CONTRIBUTING.md
  - SUPPORT.md
  - LICENSE.md
- テンプレートリポジトリ
  - リポジトリのテンプレート。テンプレートリポジトリをもとにしたリポジトリを作成することができる
  - settings > General > Template repository
- リポジトリのインサイト
- Star
  - リポジトリやトピックのお気に入り登録機能
- 機能プレビュー
  - ベータ版で利用可能な機能のリストと各機能の簡単な説明を確認し、有効無効を設定できる
- README表示の参照順
  - .github > ルート > /docs

## 3.共同作業機能 (30%)
### Issues(問題)
- GitHubにおける「タスク」を表す
- 機能
  - フィルタによる絞り込み
  - '#'によるリンクづけ
  - 担当者の割り当て
  - ブランチの作成や紐付け

- Issueの管理
  - ステータス
    - オープン
    - クローズ（Close as Complete, Close as not planned, Close as duplicated）
  - ピン留め: Issueページの先頭に表示される
  - 重複としてマーク（なぜクローズしたかわかるように、同件はマークしてクローズする）
    - Duplicate of #xx
  - 別のリポジトリに転送
  - 削除
- Issue Templates
  - 記載内容を強制することはできない
  - `.github/ISSUE_TEMPLATE/**` に格納される
  - `.md` がサポートされた拡張子
- Issue Forms
  - 書く内容を強制する
  - `.github/ISSUE_TEMPLATE/**` に格納される
  - `.yml` がサポートされた拡張子

### Pull requests
- Reviewers、Assignees
- ベースブランチ、比較ブランチ
- プルリクエストかドラフトを作成
- Conversation, Commits, Checks, Files changed
- ステータス
  - マージしてクローズ：ReOpenができない
  - マージせずクローズ：ReOpenができる
  - ドラフト：マージやレビューができない
  - オープン
- Pull Requestテンプレート
  - ルートディレクトリ、`/docs`、`.github/` に格納される
  - pull_request_template.md ファイル名はこれでないといけない
  - 上記格納場所のいずれかに、PULL_REQUEST_TEMPLATEディレクトリを作成しその配下に配置する
- キーワード
  - これらのキーワードにつづけてIssueのリンクを記述すると、PullRequestがCloseした時に連動してIssueもCloseする
  - close #xx
  - closes #xx
  - closed #xx
  - fix #xx
  - fixes #xx
  - fixed #xx
  - resolve #xx
  - resolves #xx
  - resolved #xx
- レビュー
  - Add single comment: コメント単独で投稿
  - Start a review: 複数コメントを保留しておき、まとめて投稿
- CODEOWNERSによるReviewersの指定
  - ルート/CODEOWNERS
  - CODEOWNERSで指定されたユーザがReviewersとして自動的に指定される
- 変更の提案（suggested changes）
  - コメントでコードの提案を記法に従い入力する
```
    ```suggestion
    # Sample repos
    ```
```
- レビューによるステータス
  - comment: 単にコメント（ステータスは変わらない）
  - approve: 承認する
  - request change: 変更を要求

### Discussions
- 初期で用意されているカテゴリ
  - Announcements: お知らせ
  - General: 全般
  - Ideas: アイデア
  - Polls: コミュニティから投票を受ける
  - Q&A: 質疑応答
  - Show and tell: 知見共有
- DisscussionからIssueを作成したり、ピン留めしたりできる

- Issues, Pull Request, Discussionsの違い
  - Discussions：議論や質疑応答、アイデアのフィードバックなどで利用。Issueを起こす前段階で用いる
  - Issues：タスク管理。Pull RequestやProjectsと組み合わせて複雑なタスク管理ができる
  - Pull Request：変更内容のレビュー

### Notifications
- リポジトリごと、ユーザごとに通知の設定ができる

### Gits
- コードスペニットを他の人と共有する簡単な方法
- Gitリポジトリである
- パブリック、シークレットに設定可能
  - シークレットであっても検索した際に引っかからないだけでURLを共有すれば参照できる
  - パブリックからシークレットへの変更はできない

### Wiki
- リポジトリへの書き込みアクセス権を持つユーザは、WIKIが編集可能

### GitHub Pages
- 静的サイトホスティングサービス


## 4. モダンな開発 (13%)
- GitHub Action
  - 自動化されたワークフロー
  - CICDやAPIの呼び出しができる
  - YAMLによる設定
```yaml
# イベント：トリガーを指定する
on:
  push: # Pushされたら実行
      branches:
        - main
  workflow_dispatch: # 手動で実行
# ジョブ：buildという名前のジョブを定義
jobs:
  build:
    # ubuntuの最新バージョンで実行
    runs-on: ubuntu-latest
    #  実行手順
    steps:
      - name: xxx
        uses: xxx
      - name: xxx
        uses: xxx
```

- GitHub Copilot
  - AIペアプログラマー
  - コメントをコードにしたり、テストコードを作成したりが得意

- GitHub Codespace
  - ブラウザ内に開発環境を構築する
  - ライフサイクル
    - Creating a Codespace
    - Rebulding a Codespace
    - Stopping a Codespace
      - ストレージ領域は課金がかかる
    - Deleting a Codespace
      - 課金が停止する
  - github.devエディタとの違い
    - Codespace
      - 月額使用枠を超えると有料
      - 起動に少し時間がかかる
      - コンピュートあり
      - ターミナルアクセス可能
      - たくさんの拡張機能の追加が可能
    - github.dev
      - 無料
      - 起動が瞬時（`.`で実行）
      - コンピュートなし
      - ターミナルアクセス不可能
      - 拡張機能が少しだけ

## 5. プロジェクト管理 (7%)
### GitHub Project
- IssueやPull requests をインポートしてタスク管理ができる
- 3種のレイアウトでさまざまな表示が可能
  - テーブル形式
  - カンバン形式（ボード形式）
  - ロードマップ形式
- Projectsのアクセス管理ができる
- Label：分類するために使う
- Milestones：進行状況の追跡に使う
- Projectのテンプレートも作成可能
- 返信テンプレート
  - IssueやPull requestに返信するテンプレートを自薦に作成しておくことができる
- Projectsワークフロー
- Projectsインサイト

## 6. プライバシー、セキュリティ、および管理 (10%)
### 2FA Authentication
- モバイルやデスクトップ、テキストメッセージで二要素認証の設定ができる
### GitHub Enterprise Managed Users
- ユーザはIDPで管理されるアカウント
### GitHub Administration
- リポジトリのロール：Organizationの機能
  - Read
  - Triage
  - Write
  - Maintain
  - Admin
- リポジトリの可視性
  - Public
  - Internal
  - Private
- ブランチ保護
  - リポジトリ設定で管理者のみ実行可能
  - マージとコミットの制限などができる
- 外部コラボレーター（Outside Collaborator）
  - リポジトリ管理者によって追加
  - リポジトリに直接コラボレータとして指定されるため、Teamsの参加やOrganizationに依存しない
- Organizationの権限
  - Owner(所有者)
  - Member(メンバー)
  - Moderator(モデレータ)
  - Billing manager(支払いマネージャー)
  - Security manager(セキュリティマネージャー)
  - Outside collaborator(外部コラボレータ)
- Teamの権限
  - メンバー(Member)
  - メンテナ(Maintainer)

## 7. GitHubコミュニティの利点 (10%)
### オーブンソースソフトウェアとは
- 無料である＝オープンソースではない
### GitHub Sponsors
- 支払い方法は、クレジットカード、Patreon、請求書払いがある
### GitHub Marketplace
- GitHubの開発者エコシステム
## GitおよびGitHubについて

### 変更管理の重要性
- 複数人での並行開発が当たり前
 - バッティングや上書きが発生しないよう、変更箇所や変更内容を一元管理のうえプログラムに反映させる必要がある。

- インフラのコード化
 - クラウドエンジニアもコードを書く場面が増えているので、変更管理の仕方やGitの扱いを覚えておくべき。

### Git・GitHubとは
- SVNとGit
 - どちらも変更管理システムだが、現在はGitが主流。SVNはファイル全体を保存、Gitは差分のみ保存のためGitの方が動作は速い。

- GitとGitHubの仕組み
 - 各自がローカルリポジトリで開発したプログラムを、**レビューと承認を経てから**リモートリポジトリに反映する。
  1. 現在のプログラムからブランチを切ってバグ修正等を行う `git branch hoge`
  2. ローカルリポジトリから、リモートリポジトリにプッシュする `git add hoge` `git commit -m hogehoge` `git push origin master`
  3. リモートリポジトリ上で、プルリクエストを行う（＝レビュー依頼を出す）
  4. レビューと承認を経てから、承認者によってマージを行う
 - Gitと互換性のあるリモートリポジトリがGitHub。

- GitHubのIssueとPull Request
 - Issue：課題や修正点を記録しておくもの。（Redmineのチケットのようなイメージ）
 - Pull Request：レビュー依頼を出す時に使う。イシューとの関連付け、画像や図の挿入、MarkDown記法やMermaid記法に対応。レビュアーは各行にコメントも付けられる。

#### 参照先メモ
[Gitについて](https://www.sejuku.net/blog/8224)
[Gitコマンド](https://www.sejuku.net/blog/5816)
[GitHubとのSSH接続 鍵の設定](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)
[GitHubとのSSH接続 SSH接続テスト](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/testing-your-ssh-connection)
[MarkDown記法](https://qiita.com/tbpgr/items/989c6badefff69377da7)


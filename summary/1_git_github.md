## GitとGitHub
複数人での並行開発が当たり前のため、バッティングや上書きが発生しないよう、変更箇所や変更内容を一元管理のうえプログラムに反映させる必要がある。  
また、クラウドエンジニアもコードを書く場面が増えているので、変更管理の仕方やGit・GitHubの扱いを覚える必要がある。  

### 行ったこと
Git・GitHubの設定を行い、リモートリポジトリとローカルリポジトリを作成する。

1. Git・GitHubアカウント作成
2. GitHub SSH鍵設定
3. GitHub SSH接続確認

[GitHubとのSSH接続](https://docs.github.com/ja/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account)

### メモ
#### Git・GitHubとは
- SVNとGit  
どちらも変更管理システムだが、現在はGitが主流。SVNはファイル全体を保存、Gitは差分のみ保存のためGitの方が動作は速い。  

- GitとGitHubの仕組み
各自がローカルリポジトリで開発したプログラムを、**レビューと承認を経てから**リモートリポジトリに反映する。  
  1. 現在のプログラムからブランチを切ってバグ修正等を行う `git branch hoge`
  2. ローカルリポジトリから、リモートリポジトリにプッシュする `git add hoge` `git commit -m hogehoge` `git push origin master`
  3. リモートリポジトリ上で、プルリクエストを行う（＝レビュー依頼を出す）
  4. レビューと承認を経てから、承認者によってマージを行う
  
- Gitと互換性のあるリモートリポジトリがGitHub。  

- GitHubのIssueとPull Request  
Issue：課題や修正点を記録しておくもの。 
Pull Request：レビュー依頼を出す時に使う。イシューとの関連付け可能。レビュアーは各行にコメント可能。
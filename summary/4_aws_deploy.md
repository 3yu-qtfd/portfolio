## Webアプリのデプロイ

### 行ったこと
前回構築したAWSリソース上に、Railsアプリケーションをデプロイし動作させる。

1. 組み込みサーバー（Puma）のみで起動するよう設定
2. Webサーバー（Nginx）＋APサーバー（Unicorn）で起動するよう設定
3. ALBを追加し、ALB経由でHTTP接続
4. S3を追加し、画像の保存先をローカルからS3に変更

#### 構成図
![構成図](images/4_aws_deploy/lecture05.png)

#### 1.組み込みサーバー（Puma）のみで起動するよう設定  

- タイムゾーン設定をAsia/Tokyoに変更

- パッケージ・ライブラリ類インストール
| 名称 | 備考 | 
|:-----------|:------------|
| Git       | モジュール等のcloneで使用             |
| MySQL     | MySQLクライアントのみ（RDS使用のため）|
| rbenv     | Rubyバージョン管理                    |
| Ruby      | rbenv経由でインストール               |
| Rails     | bundler経由でインストール             |
| Node.js   | nを使用しバージョンアップ             |
| yarn      |                                       |

- Pumaを起動

- ブラウザからの接続確認
3000番ポートで接続できることを確認。   
![Pumaのみで起動](images/4_aws_deploy/puma_result.PNG)


#### 2.Webサーバー（Nginx）＋APサーバー（Unicorn）で起動するよう設定
- Unicornインストール  
"unicorn.rb"を修正し、ファイルパスをワーキングディレクトリに変更。
"unicorn.pid" "unicorn.sock"の自動生成を確認。

- Nginxインストール  
リポジトリとNginx本体をインストール。
"nginx.conf"を修正し、ファイルパスをワーキングディレクトリに変更。  
  - `nginx -t`：コンフィグのシンタックスエラーを確認するコマンド。

- Nginx起動  
  - `systemctl enable nginx`：常駐化
  - `systemctl start nginx`：起動
  - `systemctl staus nginx`：ステータス確認

- Unicorn起動  
  - `bundle exec unicorn_rails -c config/unicorn.rb`：起動

- ブラウザからの接続確認  
  - HTTPのステータスコード403エラー発生。Nginxのログでパーミッションエラーあり。
    - "nginx.conf"のユーザーを"ec2-user"に変更し、解消。

  - Nginxのデフォルトページが表示される。
    - "nginx.conf"を修正し、解消。 
     ①confファイルを1つに集約　②publicディレクトリの宣言文を1つに集約  
     ③indexファイル探索の1文を明記（index.html、index.html.slim）

![Nginx+Unicornで起動](images/4_aws_deploy/unicorn-nginx_result.PNG)


#### 3. ELB経由で接続

【ポイント】ALBは、ターゲットのステータスを確認するため定期的にリクエストを送る。

- ターゲットグループの設定  
![ターゲットグループの設定](images/4_aws_deploy/alb_targetgroup.PNG)

- ロードバランサ―の設定  
![ロードバランサ―の設定](images/4_aws_deploy/alb_loadbalancer.PNG)

- ブラウザからの接続確認  
  - ブラウザからDNS名指定で接続するも、"Blocked Host"のエラーが返ってきている。  
"deveropment.rb"を編集し、該当ホストとの通信を許可するよう追記　→解消！  
![DNS名指定で起動](images/4_aws_deploy/alb_result.PNG)


#### 4. S3を追加

【ポイント】S3接続用にIAMユーザーのアクセスキーを発行し、データ保存先設定をlocalからS3に変更する。

- IAMユーザー作成・アクセスキー発行  
S3にのみ接続可能なIAMユーザーを作成し、そのアクセスキーを利用してS3に接続する。
![IAMユーザー](images/4_aws_deploy/iam_accesskey_policy.PNG)

- 画像保存先の変更  
"development.rb"を編集し、画像保存先を":local"から":amazon"に変更する。  
![画像保存先の変更](images/4_aws_deploy/developmentrb_change.PNG)

- credentialsファイルへのアクセスキー情報保存  
"development.yml.enc"を新規作成し、アクセスID・アクセスキー・バケット名を追記する。  
  -コマンド：`EDITOR=vi rails credentials:edit --environment development`  
![アクセスキー情報保存（暗号化）](images/4_aws_deploy/developmentymlenc_add.PNG)

- Image Magickインストール  
ブラウザで画像表示できるよう、EC2上へImage Magickをインストールする。

- ブラウザからの接続確認  
New fruitsボタンから画像が保存できることを確認。  
![S3追加のうえで起動](images/4_aws_deploy/S3_add_result.PNG)  

- S3への画像保存確認  
ブラウザから画像をアップロード・保存後、S3にデータが保存されていることを確認。  
![S3保存確認](images/4_aws_deploy/S3_saved.PNG)
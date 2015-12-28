# aws-tool-ami

OpsWorksから起動しているEC2インスタンスのAMIのバックアップを行うコマンドラインツールです。指定したStackの各LayerからEC2インスタンス1台分のAMIバックアップを行います。

本ツールはRubyのGemとしてパッケージングされています。

## インストール

下記のGemfileを任意のディレクトリに作成します。

```ruby
source 'https://rubygems.org'
gem "aws-tool-ami", :git => "git@github.com:petgojp/aws-tool-ami.git", :branch => "master"
```

Gemfileのあるディレクトリで下記のコマンドを実行します。

```bash
$ bundle install
```

## 事前準備

#### 設定ファイルの内容

configで渡すJSONファイルは下記の内容になります。

```json
{
  "credentials": {
    "access_key_id": "access_key_id",
    "secret_access_key": "secret_access_key"
  }
}
```

#### 適切なIAMカスタムポリシー例

上記のクレデンシャルのユーザーが持つべきポリシーを設定します。

```json
{
    "Version": "2012-10-17",
    "Statement": [
        {
            "Effect": "Allow",
            "Action": [
                "opsworks:Describe*",
                "ec2:Describe*",
                "ec2:CreateImage",
                "ec2:CreateTags",
                "ec2:DeregisterImage",
                "ec2:DeleteSnapshot"
            ],
            "Resource": [
                "*"
            ]
        }
    ]
}
```

## 利用方法

bundle exec aws-tool-amiでヘルプが表示されます。

```bash
$ bundle exec aws-tool-ami
Commands:
  aws-tool-ami create_ami [STACK_NAME] --config=CONFIG    # Create AMI of instances which belongs to specified stack
  aws-tool-ami help [COMMAND]                             # Describe available commands or one specific command
  aws-tool-ami scavenge_ami [STACK_NAME] --config=CONFIG  # Scavenge outdated AMIs which belongs to specified stack
```

#### create_ami

AMIを作成します。インスタンスの再起動は発生しません。

```
$ bundle exec aws-tool-ami create_ami corp --config ./config.json
```

#### scavenge_ami

作成後一定期間経過したAMIを削除します。

```
$ bundle exec aws-tool-ami scavenge_ami corp --config ./config.json
```

## ツールの更新について

下記のコマンドでツールを最新化します。

```bash
$ bundle update
```

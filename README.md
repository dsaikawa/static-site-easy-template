# static-site-easy-template
Simple static site templates created with CloudFormation.

## 使い方

### 事前準備

コマンドラインで実行する場合は [AWS CLI](https://docs.aws.amazon.com/ja_jp/cli/latest/userguide/getting-started-install.html) をインストールする必要があります。

本テンプレートを使用する際は以下のリソースが事前に作成されている必要があります。

- [AWS Certificate Manager](https://aws.amazon.com/jp/certificate-manager/)
  - CloudFront に 証明書を追加するため、ACM の Certificate Arn が必要です。
- [Amazon Route 53](https://aws.amazon.com/jp/route53/)
  - CloudFront のオリジナルなドメインをカスタムなドメインに変更するための DNS 登録をおこないます。

### CloudFormation 実行方法

### パラメータ

- BucketName: S3 bucket の名前、アカウントで一意になる必要がある。
- Domain: カスタムドメイン。（ex. hoge.example.com）
- HostedZoneId: 使用するドメインが登録されている Route 53 のホストゾーンID。
- AcmCertificateArn: 使用するドメインの証明書 Arn。

#### Create Stack

```shell-session
$ aws cloudformation create-stack \
  --stack-name <stack-name> \
  --template-body file://cloudformation.yml \
  --parameters ParameterKey=GroupList,ParameterValue='Users\,SimpleStaticSite' \
  ParameterKey=BucketName,ParameterValue=<bucket-name> \
  ParameterKey=Domain,ParameterValue=<domain> \
  ParameterKey=HostedZoneId,ParameterValue=<hostedZone-id> \
  ParameterKey=AcmCertificateArn,ParameterValue=<AcmCertificate-arn>
{
    "StackId": "arn:aws:cloudformation:<region>:<accountId>:stack/<stack-name>/"
}
$
```

#### Update Stack

```shell-session
$ aws cloudformation update-stack \
  --stack-name <stack-name> \
  --template-body file://cloudformation.yml \
  --parameters ParameterKey=GroupList,ParameterValue='Users\,SimpleStaticSite' \
  ParameterKey=BucketName,ParameterValue=<bucket-name> \
  ParameterKey=Domain,ParameterValue=<domain> \
  ParameterKey=HostedZoneId,ParameterValue=<hostedZone-id> \
  ParameterKey=AcmCertificateArn,ParameterValue=<AcmCertificate-arn>
{
    "StackId": "arn:aws:cloudformation:<region>:<accountId>:stack/<stack-name>/"
}
$
```

#### Delete Stack

```shell-session
$ aws cloudformation delete-stack --stack-name <stack-name>
$
```

### S3

#### Upload File

```shell-session
$ aws s3 cp index.html s3://<bucket-name>/
upload: ./index.html to s3://<bucket-name>/index.html
$
```

#### Delete File

```shell-session
$ awe s3 rm <aws-s3-file-path> # ex. s3://<bucket-name>/index.html
delete: s3://<bucket-name>/index.html
$
```
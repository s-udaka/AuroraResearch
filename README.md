# Aurora技術調査

## NLBを踏み台にAuroraへ接続

### 手順

- 基本は以下の参考記事を参考に

#### 自分がつまったところを以下に記載

- ターゲットグループ作成時のAuroraプライマリインスタンスのIPの調べ方
  - 以下のAWSの参考記事を参考にして、publicサブネットにEC2インスタンスをたて、`nslookup クラスターの書き込みエンドポイント`コマンドを叩くことでIPアドレスを取得した
- 実際にNLB経由でAuroraに接続しようとしたらタイムアウトになる（接続できない）
  - Auroraインスタンスにアタッチされているセキュリティグループのインバウンドの設定を変更した
    - NLBにアタッチしたpublicサブネット毎のEIPのプライベートIPからの5432トラフィックを許可した
      - 元々許可していたpublicサブネット自体のCIDRは削除しても接続できた（EIPから転送されているからか？）

### 構成図  

![AWS構成](https://github.com/s-udaka/AuroraResearch/blob/main/NLB%E3%82%92%E8%B8%8F%E3%81%BF%E5%8F%B0%E3%81%ABAurora%E3%81%B8%E6%8E%A5%E7%B6%9A.png "AWS構成")

### 参考記事
- [ローカルからプライベートサブネットにいるRDSに接続するためにNLBを踏み台にしてみた](https://dev.classmethod.jp/articles/nlb-bastion-for-rds/)
- [Amazon RDS DB インスタンスに接続する際の問題を解決するにはどうすればよいですか?](https://aws.amazon.com/jp/premiumsupport/knowledge-center/rds-cannot-connect/)

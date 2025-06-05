# dcm4chee-arc-psql Docker環境構築

このリポジトリは、[dcm4chee-arc-light](https://github.com/dcm4che/dcm4chee-arc-light)と[dcm4chee-arc-psql](https://hub.docker.com/r/dcm4che/dcm4chee-arc-psql)のDockerイメージを使用して、DICOM PACSシステムを簡単に構築するための`docker-compose.yml`設定を提供します。

## 概要

本プロジェクトは以下のコンポーネントを組み合わせて、DICOM PACSシステムを構築します：

1. **[dcm4chee-arc-psql](https://hub.docker.com/r/dcm4che/dcm4chee-arc-psql)**
   - DICOM PACSシステムのメインコンポーネント
   - Wildflyアプリケーションサーバー上で動作
   - Web UIとDICOMサービスを提供

2. **[PostgreSQL](https://hub.docker.com/_/postgres)**
   - データベースサーバー
   - dcm4cheeのメタデータを保存

## システム構成

### コンポーネント

1. **dcm4chee-arc-psql**
   - DICOM PACSシステムのメインコンポーネント
   - Wildflyアプリケーションサーバー上で動作
   - Web UIとDICOMサービスを提供

2. **PostgreSQL**
   - データベースサーバー
   - dcm4cheeのメタデータを保存

### ポート設定

- **8080**: Web UI (HTTP)
- **8443**: Web UI (HTTPS)
- **11112**: DICOM
- **2762**: DICOM TLS

## 環境変数

### dcm4chee

- `POSTGRES_DB`: データベース名（デフォルト: pacsdb）
- `POSTGRES_USER`: データベースユーザー（デフォルト: pacs）
- `POSTGRES_PASSWORD`: データベースパスワード（デフォルト: pacs）
- `ARCHIVE_DEVICE_NAME`: アーカイブデバイス名（デフォルト: dcm4chee-arc）
- `WILDFLY_ADMIN_USER`: Wildfly管理者ユーザー（デフォルト: admin）
- `WILDFLY_ADMIN_PASSWORD`: Wildfly管理者パスワード（デフォルト: admin）

### PostgreSQL

- `POSTGRES_DB`: データベース名（デフォルト: pacsdb）
- `POSTGRES_USER`: データベースユーザー（デフォルト: pacs）
- `POSTGRES_PASSWORD`: データベースパスワード（デフォルト: pacs）

## ボリューム

### dcm4chee

- `./data/wildfly`: Wildflyのデータディレクトリ
  - 設定ファイル
  - ログファイル
  - 一時ファイル
- `./data/archive`: DICOM画像のアーカイブディレクトリ
  - 保存されたDICOM画像
  - メタデータ

### PostgreSQL

- `./data/postgres`: PostgreSQLのデータディレクトリ
  - データベースファイル
  - トランザクションログ

## リソース制限

### dcm4chee

- メモリ: 4GB
- CPU: 2コア

### PostgreSQL

- メモリ: 2GB
- CPU: 1コア

## ヘルスチェック

### dcm4chee

- Web UIのアクセス確認
- 30秒間隔
- タイムアウト: 10秒
- リトライ: 3回

### PostgreSQL

- データベース接続確認
- 10秒間隔
- タイムアウト: 5秒
- リトライ: 5回

## 使用方法

1. リポジトリをクローン
```bash
git clone https://github.com/mochimaki/dcm4chee-arc-psql-docker.git
cd dcm4chee-arc-psql-docker
```

2. 必要なディレクトリを作成
```bash
mkdir -p data/wildfly data/archive data/postgres
```

3. コンテナを起動
```bash
docker-compose up -d
```

4. Web UIにアクセス
- URL: http://localhost:8080/dcm4chee-arc/ui2/
- ユーザー名: admin
- パスワード: admin

## 注意事項

- 本プロジェクトは、オリジナルのdcm4chee-arc-lightやdcm4chee-arc-psqlのDockerイメージを修正・拡張したものではありません。
- これらのDockerイメージを`docker-compose.yml`で組み合わせて使用するための設定ファイルを提供するものです。
- 実際のDICOM PACSシステムの機能は、オリジナルのdcm4chee-arc-lightプロジェクトによって提供されています。

## セキュリティに関する注意事項

1. 本番環境では、デフォルトのパスワードを変更してください
2. データの永続化のため、`data`ディレクトリは定期的にバックアップしてください
3. セキュリティのため、必要に応じてファイアウォールでポートを制限してください

## トラブルシューティング

1. コンテナのログを確認
```bash
docker-compose logs -f dcm4chee
```

2. コンテナの状態を確認
```bash
docker-compose ps
```

3. コンテナを再起動
```bash
docker-compose restart dcm4chee
```

## ライセンス

dcm4chee-arc-psqlは[Apache License 2.0](https://www.apache.org/licenses/LICENSE-2.0)の下で提供されています。 
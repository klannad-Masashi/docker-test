# Docker Web System

フロントエンド（Angular）、バックエンド（Spring Boot）、データベース（PostgreSQL）のDocker構成によるウェブシステム

## システム構成

- **フロントエンド**: Angular
- **バックエンド**: Spring Boot
- **データベース**: PostgreSQL
- **オーケストレーション**: Docker Compose

## プロジェクト構造

```
docker-test/
├── frontend/                    # Angular フロントエンドアプリケーション
│   ├── src/                     # Angular ソースコード
│   │   ├── app/                 # アプリケーションメインコード
│   │   │   ├── components/      # Angular コンポーネント
│   │   │   ├── services/        # Angular サービス
│   │   │   ├── models/          # データモデル
│   │   │   └── app.module.ts    # メインモジュール
│   │   ├── assets/              # 静的ファイル（画像、CSS等）
│   │   ├── environments/        # 環境設定ファイル
│   │   └── index.html           # メインHTMLファイル
│   ├── angular.json             # Angular CLI設定
│   ├── package.json             # Node.js依存関係
│   ├── tsconfig.json            # TypeScript設定
│   ├── proxy.conf.json          # 開発用プロキシ設定
│   └── Dockerfile               # フロントエンド用Dockerファイル
│
├── backend/                     # Spring Boot バックエンドアプリケーション
│   ├── src/
│   │   └── main/
│   │       ├── java/
│   │       │   └── com/example/webapp/
│   │       │       ├── controller/     # REST API コントローラ
│   │       │       ├── service/        # ビジネスロジック
│   │       │       ├── repository/     # データアクセス層
│   │       │       ├── entity/         # JPA エンティティ
│   │       │       ├── dto/            # データ転送オブジェクト
│   │       │       ├── config/         # 設定クラス
│   │       │       └── WebappApplication.java # メインクラス
│   │       └── resources/
│   │           ├── application.yml     # Spring Boot設定
│   │           ├── application-dev.yml # 開発環境設定
│   │           └── data.sql            # 初期データ（オプション）
│   ├── build.gradle             # Gradle ビルド設定
│   ├── settings.gradle          # Gradle プロジェクト設定
│   ├── gradle/                  # Gradle Wrapper
│   │   └── wrapper/
│   ├── gradlew                  # Gradle Wrapper (Unix)
│   ├── gradlew.bat              # Gradle Wrapper (Windows)
│   └── Dockerfile               # バックエンド用Dockerファイル
│
├── database/                    # データベース関連ファイル
│   ├── init/                    # DB初期化スクリプト
│   │   ├── 01-create-database.sql      # データベース作成
│   │   ├── 02-create-tables.sql        # テーブル作成
│   │   └── 03-insert-data.sql          # 初期データ投入
│   └── Dockerfile               # PostgreSQL用Dockerファイル（カスタム）
│
├── docker-compose.yml           # Docker Compose メイン設定
├── docker-compose.dev.yml       # 開発環境用オーバーライド
├── docker-compose.prod.yml      # 本番環境用オーバーライド
├── .env                         # 環境変数ファイル
├── .env.example                 # 環境変数テンプレート
├── .gitignore                   # Git除外ファイル
└── README.md                    # このファイル
```

## 前提条件

- Docker Desktop がインストールされていること
- Docker Compose が利用可能であること

## 技術スタック

### フロントエンド
- **Node.js**: 20.11.0
- **Angular**: 17.x
- **TypeScript**: 5.4.x

### バックエンド
- **Java**: Azul JDK 21
- **Spring Boot**: 3.x
- **Gradle**: 8.x

### データベース
- **PostgreSQL**: 15.x

## セットアップ手順

### 1. リポジトリのクローン

```bash
git clone <リポジトリURL>
cd docker-test
```

### 2. アプリケーションの起動

```bash
docker compose up -d
```

### 3. アクセス確認

- フロントエンド: http://localhost:4200
- バックエンド API: http://localhost:8080
- データベース: localhost:5432

## 開発時の操作

### ログの確認

```bash
# 全サービスのログ
docker compose logs -f

# 特定のサービスのログ
docker compose logs -f frontend
docker compose logs -f backend
docker compose logs -f database
```

### サービスの再起動

```bash
# 特定のサービスの再起動
docker compose restart backend

# 全サービスの再起動
docker compose restart
```

### 開発環境でのホットリロード

フロントエンドとバックエンドはソースコードの変更を検知して自動的に再ビルドされます。

## データベース接続情報

- **ホスト**: localhost (コンテナ内では `database`)
- **ポート**: 5432
- **データベース名**: webapp
- **ユーザー名**: postgres
- **パスワード**: password

## 停止とクリーンアップ

```bash
# サービスの停止
docker compose down

# ボリューム含めて完全削除
docker compose down -v

# イメージも含めて削除
docker compose down --rmi all -v
```

## トラブルシューティング

### ポートが既に使用されている場合

`docker-compose.yml` のポート設定を変更してください。

### データベース接続エラー

1. データベースコンテナが起動しているか確認
2. 接続情報が正しいか確認
3. ファイアウォール設定を確認

### フロントエンドがバックエンドに接続できない場合

- プロキシ設定（`proxy.conf.json`）を確認
- CORS設定を確認

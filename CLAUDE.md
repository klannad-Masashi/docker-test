# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## プロジェクト概要

Docker構成によるフルスタックWebアプリケーション開発プロジェクト：
- **フロントエンド**: Angular 17.x (Node.js 20.11.0) - 未実装
- **バックエンド**: Spring Boot 3.x (Java 21) - ✅ 実装完了
- **オーケストレーション**: Docker Compose - ✅ 実装完了

**重要**: このプロジェクトではすべてコンテナで実行します。ローカルでのJavaやNode.js実行は禁止です。

## 開発コマンド

### Docker環境
```bash
# バックエンドサービス起動
docker compose up -d

# バックエンド再起動  
docker compose restart backend

# ログ確認
docker compose logs -f backend

# 環境停止・削除
docker compose down

# APIテスト
curl http://localhost:8080/api/hello  # "helloworld" が返されること
```

### バックエンド（Spring Boot）
実装済みのバックエンドコマンド（コンテナ内実行）：
```bash
# ビルド（コンテナ内）
docker run --rm -v $(pwd)/backend:/app gradle:8.5-jdk21 bash -c "cd /app && gradle build"

# テスト実行（コンテナ内）  
docker run --rm -v $(pwd)/backend:/app gradle:8.5-jdk21 bash -c "cd /app && gradle test"

# コンテナ内でコマンド実行
docker exec webapp-backend [command]
```

## アーキテクチャ構成

### 現在の実装状況
- ✅ バックエンド（Spring Boot）: 実装完了・テスト済み
- ✅ Docker Compose: 実装完了
- ⏳ フロントエンド（Angular）: 未実装

### サービス構成
現在の構成：

1. **バックエンド**: Spring Boot REST API
   - Hello World API実装済み (`/api/hello` → `helloworld`)
   - CORS設定済み
   - Docker化完了
   - ヘルスチェック対応

### ネットワーク構成
- バックエンド API: localhost:8080
- コンテナネットワーク: webapp-network

## 実装計画の参照

詳細な実装フェーズについては `PLAN.md` を参照。現在はフェーズ1（基盤構築）完了。

✅ 完了した作業：
1. Docker Compose設定
2. Spring Boot Hello World API実装
3. Docker化とコンテナ実行環境構築

次の実装優先順位：
1. Angular フロントエンド実装
2. フロントエンド・バックエンド連携

## 開発時の注意点

### 必須ルール
- **すべてコンテナで実行** - ローカル実行禁止
- ホストマシンにJavaやNode.js環境は不要
- Docker Composeですべてを管理

### バックエンド
- Spring Boot 3.x + Java 21
- Gradle 8.5でビルド
- CORS設定済み（localhost:4200対応）
- 環境変数での設定分離（.env使用）
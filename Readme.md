# 🏢 IoT監視システム (Facility Monitoring System)

建屋の電力・温湿度データをリアルタイム監視・可視化するシステム

## 📊 システム概要

### 主な機能
- **CSV自動監視**: IoTセンサーから出力されるCSVファイルを自動検出・処理
- **データ正規化**: ワイド形式からロング形式へのデータ変換
- **リアルタイム可視化**: Apache Supersetによるダッシュボード
- **時系列データ最適化**: PostgreSQL + TimescaleDBによる高速処理

### システム構成
```
IoTセンサー → CSV出力 → 自動監視 → DB格納 → 可視化ダッシュボード
```

---

## 📁 ディレクトリ構造と役割

### 🔧 **src/** - ソースコードディレクトリ
メインプログラムのソースコードを格納

#### **src/csv_monitor/** - CSV監視システム
```
役割: CSVファイルの監視・処理・データベース挿入
含まれるファイル:
  ├── csv_processor.py      # メイン処理ロジック
  ├── file_watcher.py       # ファイル監視（Watchdog）
  ├── data_normalizer.py    # ワイド→ロング形式変換
  └── database_client.py    # データベース操作
```

#### **src/database/** - データベース管理
```
役割: データベーススキーマ管理・マイグレーション
含まれるファイル:
  ├── migrations/           # SQLマイグレーションファイル
  │   ├── 001_create_tables.sql
  │   └── 002_create_indexes.sql
  ├── models.py             # データモデル定義
  └── connection.py         # DB接続管理
```

#### **src/superset/** - ダッシュボード設定
```
役割: Apache Superset設定・ダッシュボード定義
含まれるファイル:
  ├── superset_config.py    # Superset設定ファイル
  ├── dashboards/           # ダッシュボード定義JSON
  └── datasets/             # データセット定義
```

#### **src/services/** - Windowsサービス
```
役割: システムの常駐サービス化
含まれるファイル:
  ├── csv_monitor_service.py  # Windows Service本体
  └── service_installer.py    # サービス登録スクリプト
```

#### **src/utils/** - 共通ユーティリティ
```
役割: 各モジュール共通の機能
含まれるファイル:
  ├── logger.py             # ログ設定
  ├── config_loader.py      # 設定ファイル読み込み
  └── validators.py         # データバリデーション
```

---

### ⚙️ **config/** - 設定ファイル
```
役割: システム全体の設定管理
含まれるファイル:
  ├── database.yaml         # データベース接続設定
  ├── monitoring.yaml       # 監視システム設定
  ├── superset_config.py    # Superset詳細設定
  └── nginx.conf            # Webサーバー設定
```

---

### 💾 **data/** - データ格納ディレクトリ

#### **data/csv_input/** - CSV入力フォルダ
```
役割: IoTセンサーからのCSVファイル受信
説明: 監視対象フォルダ。ここにCSVファイルが保存されると自動処理開始
ファイル例:
  ├── Building_A_20241225.csv
  ├── Building_B_20241225.csv
  └── Building_C_20241225.csv
```

#### **data/csv_processed/** - 処理済みCSV
```
役割: 処理完了したCSVファイルの保管
説明: 正常処理されたCSVファイルをタイムスタンプ付きで移動
ファイル例:
  ├── processed_20241225_143022_Building_A_20241225.csv
  └── processed_20241225_143105_Building_B_20241225.csv
```

#### **data/csv_error/** - エラーCSV
```
役割: 処理エラーが発生したCSVファイルの保管
説明: フォーマットエラーや読み込み失敗したCSVファイル
ファイル例:
  ├── error_20241225_143022_Invalid_Data.csv
  └── error_20241225_144005_Format_Error.csv
```

#### **data/sample_data/** - サンプル・テストデータ
```
役割: 開発・テスト用のサンプルCSVファイル
含まれるファイル:
  ├── Building_A_sample.csv     # 建屋Aのサンプルデータ
  ├── Building_B_sample.csv     # 建屋Bのサンプルデータ
  └── test_data_generator.py    # テストデータ生成スクリプト
```

---

### 📋 **logs/** - ログファイル
```
役割: システム動作ログの保存
含まれるファイル:
  ├── csv_monitor.log       # CSV監視システムログ
  ├── superset.log          # Supersetダッシュボードログ
  ├── database.log          # データベース操作ログ
  └── system.log            # システム全体ログ
```

---

### 🚀 **scripts/** - 運用スクリプト
```
役割: セットアップ・運用・保守用スクリプト
含まれるファイル:
  ├── setup_database.py      # データベース初期化
  ├── install_services.py    # Windowsサービス登録
  ├── start_superset.py      # Superset起動
  ├── backup_database.py     # データベースバックアップ
  └── health_check.py        # システム死活監視
```

---

### 🧪 **tests/** - テストファイル
```
役割: 単体テスト・統合テストコード
含まれるファイル:
  ├── test_csv_processor.py     # CSV処理テスト
  ├── test_data_normalizer.py   # データ正規化テスト
  ├── test_database.py          # データベーステスト
  └── test_integration.py       # 統合テスト
```

---

### 📚 **docs/** - ドキュメント
```
役割: システム設計書・運用マニュアル
含まれるファイル:
  ├── setup_guide.md           # セットアップ手順書
  ├── operation_manual.md      # 運用マニュアル
  ├── api_reference.md         # API仕様書
  ├── troubleshooting.md       # トラブルシューティング
  └── architecture.md          # システム設計書
```

---

### 🔧 **.vscode/** - VSCode設定
```
役割: Visual Studio Code開発環境設定
含まれるファイル:
  ├── settings.json         # エディタ設定
  ├── launch.json           # デバッグ設定
  ├── tasks.json            # タスク設定
  └── extensions.json       # 推奨拡張機能リスト
```

---

## 🚀 クイックスタート

### 1. 環境セットアップ
```powershell
# 依存関係インストール
pip install -r requirements.txt

# データベース初期化
python scripts/setup_database.py
```

### 2. システム起動
```powershell
# CSV監視システム起動
python src/csv_monitor/csv_processor.py

# Supersetダッシュボード起動
python scripts/start_superset.py
```

### 3. テストデータ投入
```powershell
# サンプルデータ生成
python data/sample_data/test_data_generator.py
```

---

## 📊 データフロー

```
1. IoTセンサー → CSV出力 → data/csv_input/
2. file_watcher.py → CSVファイル検出
3. csv_processor.py → データ読み込み・正規化
4. database_client.py → PostgreSQL/TimescaleDBに挿入
5. Apache Superset → データ可視化
6. 処理済みCSV → data/csv_processed/ に移動
```

---

## 🛠️ 主要コンポーネント

### データベース（PostgreSQL + TimescaleDB）
- **用途**: 時系列センサーデータの高速格納・検索
- **特徴**: 自動パーティショニング、データ圧縮、高速集約

### CSV監視システム（Python + Watchdog）
- **用途**: リアルタイムファイル監視・自動処理
- **特徴**: ワイド→ロング形式変換、データバリデーション

### ダッシュボード（Apache Superset）
- **用途**: インタラクティブな可視化・分析
- **特徴**: リアルタイムグラフ、ドリルダウン分析

---

## 🔒 セキュリティ

### ネットワーク分離
- IoT系ネットワークとOA系ネットワークの分離
- UTMによるファイアウォール保護

### データ保護
- HTTPS通信での暗号化
- データベースアクセス制御
- ログファイルの適切な管理

---

## 📈 パフォーマンス目標

- **CSV処理速度**: 3-5秒/ファイル（50MB）
- **クエリ応答時間**: 1-3秒（年間データ検索）
- **データ圧縮率**: 70-80%（TimescaleDB効果）
- **同時接続数**: 50ユーザー

---

## 🔧 開発・運用

### 開発環境
- **IDE**: Visual Studio Code
- **言語**: Python 3.11+
- **デバッグ**: VSCode統合デバッガー

### 本番環境
- **OS**: Windows 11 Professional
- **サービス**: Windows Service常駐
- **監視**: ログベース監視

---

## 📞 サポート・トラブルシューティング

### ログ確認
```powershell
# システムログ確認
Get-Content logs/csv_monitor.log -Tail 50

# エラーログ確認
Get-Content logs/system.log | Select-String "ERROR"
```

### 基本的な問題解決
1. **CSV処理が動かない** → `logs/csv_monitor.log` 確認
2. **データベース接続エラー** → PostgreSQLサービス状態確認
3. **ダッシュボード表示されない** → Supersetログ確認

詳細は `docs/troubleshooting.md` を参照

---

## 📝 更新履歴

| バージョン | 日付 | 変更内容 |
|-----------|------|---------|
| 1.0.0 | 2024-12-25 | 初期リリース |

---

## 👥 開発チーム

- **システム設計**: Claude AI Assistant
- **実装・運用**: 施設管理チーム
# Java Development with Docker

このプロジェクトはDockerを使用してJava開発環境を提供します。ローカル環境にJavaをインストールすることなく、Dockerコンテナ内でJavaプログラムのコンパイルと実行が可能です。

## 必要な環境

- Docker
- Docker Compose

## セットアップ

1. Dockerイメージをビルドしてコンテナを起動:
```bash
docker-compose up -d --build
```

## 使用方法

### コンテナ内でシェルを起動

```bash
docker-compose exec java-dev bash
```

### Javaプログラムのコンパイルと実行

#### 方法1: コンテナ内で直接実行

1. コンテナ内のシェルに入る:
```bash
docker-compose exec java-dev bash
```

2. Javaファイルをコンパイル:
```bash
cd /app
javac -d out src/HelloWorld.java
```

3. プログラムを実行:
```bash
java -cp out HelloWorld
```

#### 方法2: ホストから直接実行

HelloWorldの例:
```bash
docker-compose exec java-dev javac -d out src/HelloWorld.java
docker-compose exec java-dev java -cp out HelloWorld
```

Calculator（電卓）の例:
```bash
docker-compose exec java-dev javac -d out src/Calculator.java
docker-compose exec java-dev java -cp out Calculator 10 + 5
docker-compose exec java-dev java -cp out Calculator 20 "*" 3
```

### ディレクトリ構造

```
java-dev-docker/
├── Dockerfile         # Dockerイメージの定義
├── docker-compose.yml # Docker Composeの設定
├── README.md         # このファイル
├── .gitignore        # Gitで無視するファイルの設定
├── src/              # Javaソースコード
│   ├── HelloWorld.java
│   └── Calculator.java
└── out/              # コンパイル済みクラスファイル（自動生成）
```

## サンプルプログラム

### HelloWorld.java
基本的な"Hello, World!"プログラム

### Calculator.java
コマンドライン引数を使った簡単な電卓プログラム

使用例:
```bash
java -cp out Calculator 10 + 5    # 10 + 5 = 15
java -cp out Calculator 20 - 8    # 20 - 8 = 12
java -cp out Calculator 6 "*" 7   # 6 * 7 = 42
java -cp out Calculator 15 / 3    # 15 / 3 = 5
```

## コンテナの停止と削除

```bash
# コンテナを停止
docker-compose stop

# コンテナを停止して削除
docker-compose down

# イメージも含めて削除
docker-compose down --rmi all
```

## トラブルシューティング

### パーミッションエラーが発生する場合
ホストとコンテナ間のユーザーIDが異なる場合、ファイルのパーミッションエラーが発生することがあります。その場合は、Dockerfileを修正してユーザーIDを合わせるか、ファイルのパーミッションを適切に設定してください。

### Javaバージョンを変更したい場合
`Dockerfile`の`FROM`行を編集して、必要なJavaバージョンのイメージに変更してください。例:
- Java 11: `FROM openjdk:11-slim`
- Java 8: `FROM openjdk:8-slim`
- Java 21: `FROM openjdk:21-slim`
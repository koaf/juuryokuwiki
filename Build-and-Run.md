# Build and Run - ビルドと実行方法

## 📦 ビルド方法

### バッチファイル使用（推奨）

```batch
@compile.bat
```

このバッチファイルは以下を実行します：
```batch
javac Main.java
pause
```

### コマンドライン

```batch
# 全Javaファイルをコンパイル
javac *.java

# または Main.java のみ（依存関係は自動解決）
javac Main.java

# 文字コード指定（必要に応じて）
javac -encoding UTF-8 *.java
```

## ▶️ 実行方法

### バッチファイル使用（推奨）

```batch
@start.bat
```

このバッチファイルは以下を実行します：
```batch
java Main
```

### コマンドライン

```batch
java Main
```

## 📚 Javadoc生成

### バッチファイル使用

```batch
@javadoc.bat
```

このバッチファイルは以下を実行します：
```batch
javadoc -d doc -encoding UTF-8 -private -author -version *.java
```

### オプション説明

| オプション | 説明 |
|------------|------|
| `-d doc` | 出力先ディレクトリ |
| `-encoding UTF-8` | ソースファイルの文字コード |
| `-private` | privateメンバーも含める |
| `-author` | @authorタグを含める |
| `-version` | @versionタグを含める |

### 生成されたドキュメント

- `doc/index.html` - トップページ
- `doc/[ClassName].html` - 各クラスのドキュメント
- `doc/package-summary.html` - パッケージ概要

## 🔧 IDE での実行

### Eclipse

1. 「ファイル」→「開く」→プロジェクトフォルダを選択
2. `Main.java` を右クリック
3. 「実行」→「Javaアプリケーション」

### IntelliJ IDEA

1. 「ファイル」→「開く」→プロジェクトフォルダを選択
2. `Main.java` を右クリック
3. 「Run 'Main.main()'」

### Visual Studio Code

1. Java Extension Pack をインストール
2. フォルダを開く
3. `Main.java` を開き、`Run` をクリック

## 🎓 トレーニングの実行

### Q学習トレーニング

```batch
# コンパイル
javac QLearningTrainer.java

# 実行
java QLearningTrainer
```

- Ctrl+C で中断可能（学習データは自動保存）
- 学習データは `qlearning_model.dat` に保存

### モデル検証

```batch
javac ModelValidator.java
java ModelValidator qlearning_model.dat
```

### モデル最適化

```batch
javac ModelOptimizer.java QLearningPlayerV2.java
java ModelOptimizer qlearning_model.dat qlearning_optimized.dat 0.0001
```

## 📁 出力ファイル

### コンパイル結果

```
*.class  - コンパイルされたバイトコード
```

### ゲーム実行時

```
log/YYYYMMDD_HHmmss.txt  - 棋譜ファイル
```

### トレーニング実行時

```
qlearning_model.dat  - Q学習の学習済みモデル
```

## ⚠️ よくある問題と解決方法

### 文字化け

```batch
# コンパイル時にエンコーディングを指定
javac -encoding UTF-8 *.java
```

### クラスが見つからない

```batch
# カレントディレクトリを確認
cd /d [プロジェクトフォルダ]

# クラスパスを明示的に指定
java -cp . Main
```

### 画像が表示されない

- `image/` フォルダが存在するか確認
- 画像ファイル名が正しいか確認

### 音が鳴らない

- `sound/` フォルダが存在するか確認
- `fall.wav` が存在するか確認
- 音声形式がサポートされているか確認

---

[← File Structure](File-Structure.md) | [Creating Custom AI →](Creating-Custom-AI.md)

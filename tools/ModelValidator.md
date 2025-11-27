# ModelValidator クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `ModelValidator.java` |
| **役割** | Q学習モデルファイルの検証 |
| **種別** | ツールクラス |

## 📝 説明

Q-Learningモデルファイルの検証ツール。ファイルが破損していないか、正しく読み込めるかをチェックします。

## 🎮 使い方

### コマンドライン

```batch
# コンパイル
javac ModelValidator.java

# 実行
java ModelValidator qlearning_model.dat
```

## 🔧 メソッド

### main(String[] args)

```java
public static void main(String[] args)
```

エントリーポイント。

**パラメータ:**
- `args[0]` - 検証するモデルファイルのパス

### validateModel(String modelPath)

```java
public static void validateModel(String modelPath)
```

モデルファイルを検証します。

**パラメータ:**
- `modelPath` - モデルファイルのパス

**検証内容:**
1. ファイル存在チェック
2. ファイルサイズチェック
3. デシリアライズ可能かチェック
4. Q-tableの統計情報を表示

## 📊 出力例

```
========================================
Q-Learning Model Validator
========================================
File: C:\path\to\qlearning_model.dat
✓ File exists
✓ File size: 1,234,567 bytes (1.18 MB)
✓ Deserialization successful
========================================
Q-Table Statistics:
  Total states: 12,345
  Sample state Q-values:
    State: 0|111111111...
    Q-values: [0.12, 0.45, 0.67, 0.23, 0.15, 0.34, 0.56]
========================================
Validation PASSED
========================================
```

## 📄 ソースコード

```java
public class ModelValidator {
    
    public static void main(String[] args) {
        if (args.length < 1) {
            System.out.println("Usage: java ModelValidator <model.dat>");
            return;
        }
        
        String modelPath = args[0];
        validateModel(modelPath);
    }

    @SuppressWarnings("unchecked")
    public static void validateModel(String modelPath) {
        File file = new File(modelPath);
        
        System.out.println("========================================");
        System.out.println("Q-Learning Model Validator");
        System.out.println("========================================");
        System.out.println("File: " + file.getAbsolutePath());
        
        // ファイル存在チェック
        if (!file.exists()) {
            System.err.println("✗ File does not exist!");
            return;
        }
        System.out.println("✓ File exists");
        
        // ファイルサイズチェック
        long fileSize = file.length();
        // ... 続く
    }
}
```

## 🔍 検証項目

| チェック | 説明 |
|----------|------|
| ファイル存在 | 指定パスにファイルがあるか |
| ファイルサイズ | 0バイトでないか、異常に大きくないか |
| デシリアライズ | `ObjectInputStream`で読み込めるか |
| 型チェック | `Map<String, double[]>`形式か |
| 整合性 | Q値配列が正しいサイズか |

## 💡 使用シーン

- 学習後のモデルファイルが正しく保存されたか確認
- 別環境にモデルを移動した後の検証
- エラー発生時の原因切り分け

## 🔗 関連クラス

- [QLearningPlayerV2](../ai/QLearningPlayerV2.md) - モデルを使用するクラス
- [QLearningTrainer](../ai/QLearningTrainer.md) - モデルを生成するクラス
- [ModelOptimizer](ModelOptimizer.md) - モデルを最適化するツール

---

[← QLearningTrainer](../ai/QLearningTrainer.md) | [ModelOptimizer →](ModelOptimizer.md)

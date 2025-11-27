# ModelOptimizer クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `ModelOptimizer.java` |
| **役割** | Q学習モデルの最適化（圧縮） |
| **種別** | ツールクラス |

## 📝 説明

Q-Learningモデルを最適化（圧縮）するツール。使用頻度の低い状態を削除してファイルサイズを削減します。

## 🎮 使い方

### コマンドライン

```batch
# コンパイル
javac ModelOptimizer.java QLearningPlayerV2.java

# 実行
java ModelOptimizer <入力ファイル> <出力ファイル> [閾値]

# 例
java ModelOptimizer qlearning_model.dat qlearning_optimized.dat 0.0001
```

### パラメータ

| パラメータ | 説明 | デフォルト |
|------------|------|-----------|
| 入力ファイル | 最適化するモデルファイル | （必須） |
| 出力ファイル | 最適化後の保存先 | （必須） |
| 閾値 | 保持する最小Q値 | 0.0001 |

## 🔧 メソッド

### main(String[] args)

```java
public static void main(String[] args)
```

エントリーポイント。

### optimize(String inputPath, String outputPath, double threshold)

```java
public static void optimize(String inputPath, String outputPath, double threshold) 
    throws IOException, ClassNotFoundException
```

モデルを最適化します。

**パラメータ:**
- `inputPath` - 入力モデルファイルのパス
- `outputPath` - 出力モデルファイルのパス
- `threshold` - 保持する最小Q値

## 📊 最適化アルゴリズム

### 削除条件

各状態について、すべてのQ値が閾値未満の場合にその状態を削除：

```java
boolean allBelowThreshold = true;
for (double qval : qvals) {
    if (Math.abs(qval) >= threshold) {
        allBelowThreshold = false;
        break;
    }
}
if (allBelowThreshold) {
    // この状態を削除
}
```

### 処理フロー

```
1. 入力モデルを読み込み
2. 各状態をチェック
   - すべてのQ値 < 閾値 → 削除
   - それ以外 → 保持
3. 最適化されたモデルを保存
4. 統計を表示
```

## 📄 出力例

```
========================================
Q-Learning Model Optimizer
========================================
Input:  qlearning_model.dat
Output: qlearning_optimized.dat
Threshold: 0.0001
========================================
Original states: 50,000
Removed states:  35,000 (70.0%)
Remaining states: 15,000
========================================
Original size:  5.23 MB
Optimized size: 1.57 MB
Compression:    70.0%
========================================
Optimization complete!
```

## 💡 閾値の選び方

| 閾値 | 効果 | 推奨用途 |
|------|------|----------|
| 0.0001 | 軽度圧縮 | 精度重視 |
| 0.001 | 中程度圧縮 | バランス |
| 0.01 | 強圧縮 | サイズ重視 |

**注意:** 閾値を大きくすると圧縮率は上がりますが、学習した知識が失われる可能性があります。

## 🔧 使用例

### 基本的な最適化

```batch
java ModelOptimizer qlearning_model.dat qlearning_optimized.dat
```

### 積極的な圧縮

```batch
java ModelOptimizer qlearning_model.dat qlearning_small.dat 0.01
```

### 慎重な圧縮

```batch
java ModelOptimizer qlearning_model.dat qlearning_careful.dat 0.00001
```

## 📁 入出力ファイル

| ファイル | 説明 |
|----------|------|
| 入力 | `QLearningPlayerV2`で保存されたモデルファイル |
| 出力 | 最適化されたモデルファイル（同じ形式） |

最適化後のファイルは `QLearningPlayerV2.loadQ()` で読み込み可能です。

## 🔗 関連クラス

- [QLearningPlayerV2](../ai/QLearningPlayerV2.md) - モデルを使用するクラス
- [QLearningTrainer](../ai/QLearningTrainer.md) - モデルを生成するクラス
- [ModelValidator](ModelValidator.md) - モデルを検証するツール

---

[← ModelValidator](ModelValidator.md) | [Recoder →](Recoder.md)

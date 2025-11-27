# Creating Custom AI - オリジナルAIの作成方法

## 🎯 課題の目標

`Com.java` の `put()` メソッドを改良して、より強いAIを作成することが課題の主目的です。

## 📝 基本構造

### Com.java の put() メソッド

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();  // 現在の盤面を取得
    int putColumn;

    //=======↓↓↓　TODO: 以下の部分のプログラムを改造してください。　↓↓↓======
    
    // ここにAIロジックを実装
    MonteCarloPlayer mc = new MonteCarloPlayer(100);
    putColumn = mc.selectMove(grid, this.order);

    //=======↑↑↑　以上の部分にプログラミングしてください　↑↑↑======
    return putColumn;
}
```

### 利用可能な情報

| 変数/メソッド | 型 | 説明 |
|---------------|------|------|
| `grid` | `int[][]` | 盤面データ（6行×7列） |
| `this.order` | `int` | 自分の順番（0:先手, 1:後手） |
| `judge.canPut(col)` | `int` | 列colに置けるかチェック |

### 盤面データの形式

```java
grid[行][列]
// -1: 空（石なし）
//  0: 先手の石
//  1: 後手の石
```

## 🔰 レベル1: ランダムAI

最もシンプルなAIです。

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int putColumn;
    
    // ランダムに列を選ぶ
    java.util.Random rnd = new java.util.Random();
    do {
        putColumn = rnd.nextInt(7);  // 0～6からランダム
    } while (judge.canPut(putColumn) == -1);  // 置けない場合は再選択
    
    return putColumn;
}
```

## 🔹 レベル2: リーチ優先AI

自分または相手がリーチ（3連続）している場合に対応します。

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int putColumn = -1;
    
    // 1. 自分がリーチなら勝つ
    for (int col = 0; col < 7; col++) {
        if (judge.canPut(col) != -1) {
            if (checkWinningMove(grid, col, this.order)) {
                return col;
            }
        }
    }
    
    // 2. 相手がリーチなら止める
    int opponent = 1 - this.order;  // 相手の番号
    for (int col = 0; col < 7; col++) {
        if (judge.canPut(col) != -1) {
            if (checkWinningMove(grid, col, opponent)) {
                return col;
            }
        }
    }
    
    // 3. それ以外はランダム
    java.util.Random rnd = new java.util.Random();
    do {
        putColumn = rnd.nextInt(7);
    } while (judge.canPut(putColumn) == -1);
    
    return putColumn;
}

// 指定列に置いたら勝てるかチェック
private boolean checkWinningMove(int[][] grid, int col, int player) {
    // 置ける行を探す
    int row = -1;
    for (int r = 5; r >= 0; r--) {
        if (grid[r][col] == -1) {
            row = r;
            break;
        }
    }
    if (row == -1) return false;
    
    // 一時的に石を置いて4連チェック
    // （実装は省略 - 横縦斜めをチェック）
    return false;  // 実際はここで判定
}
```

## 🔷 レベル3: 評価関数AI

盤面の状況を評価して、最良の手を選びます。

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int bestCol = 0;
    int bestScore = Integer.MIN_VALUE;
    
    for (int col = 0; col < 7; col++) {
        if (judge.canPut(col) != -1) {
            int score = evaluateMove(grid, col, this.order);
            if (score > bestScore) {
                bestScore = score;
                bestCol = col;
            }
        }
    }
    
    return bestCol;
}

// 手の評価
private int evaluateMove(int[][] grid, int col, int player) {
    int score = 0;
    
    // 中央寄りの列を優先（+10 ~ +40）
    int centerBonus = 40 - Math.abs(col - 3) * 10;
    score += centerBonus;
    
    // リーチを作れるなら高得点
    // 相手のリーチを止めるなら高得点
    // 連続した石があれば加点
    // など...
    
    return score;
}
```

## ⭐ レベル4: モンテカルロ法

ランダムシミュレーションで勝率を計算します。

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    
    // MonteCarloPlayer を使用（シミュレーション回数を指定）
    MonteCarloPlayer mc = new MonteCarloPlayer(100);  // 1手100回シミュレーション
    return mc.selectMove(grid, this.order);
}
```

### パラメータ調整

```java
// シミュレーション回数を増やすと精度向上（時間も増加）
new MonteCarloPlayer(50);   // 高速・低精度
new MonteCarloPlayer(100);  // 標準
new MonteCarloPlayer(500);  // 低速・高精度
```

## 🌟 レベル5: Q学習AI

強化学習で学習済みのモデルを使用します。

### 学習済みモデルがある場合

```java
private QLearningPlayerV2 qlearner = null;

public Com(int order) {
    this(...);  // コンストラクタ
    
    // 学習済みモデルを読み込み
    try {
        qlearner = new QLearningPlayerV2(6, 7);
        qlearner.loadQ("qlearning_model.dat");
    } catch (Exception e) {
        qlearner = null;
    }
}

@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    
    if (qlearner != null) {
        // Q学習を使用（epsilon=0.01で少し探索）
        return qlearner.selectMove(grid, this.order, 0.01);
    } else {
        // フォールバック：モンテカルロ
        MonteCarloPlayer mc = new MonteCarloPlayer(100);
        return mc.selectMove(grid, this.order);
    }
}
```

### モデルの学習方法

```batch
# 学習プログラムを実行
java QLearningTrainer

# Ctrl+C で中断（自動保存される）
```

## 🔧 ハイブリッドAI

複数のアプローチを組み合わせることも可能です。

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    
    // 1. まず勝てるかチェック
    int winMove = findWinningMove(grid, this.order);
    if (winMove != -1) return winMove;
    
    // 2. 相手のリーチを止める
    int blockMove = findWinningMove(grid, 1 - this.order);
    if (blockMove != -1) return blockMove;
    
    // 3. それ以外はモンテカルロで決める
    MonteCarloPlayer mc = new MonteCarloPlayer(50);
    return mc.selectMove(grid, this.order);
}
```

## 📋 チェックリスト

AIを実装する際のチェックポイント：

- [ ] 合法手のみを返している
- [ ] 境界チェックを行っている
- [ ] 無限ループにならない
- [ ] 適切な時間内に応答する
- [ ] 例外処理を行っている

## 🎮 テスト方法

1. コンパイル: `@compile.bat`
2. 実行: `@start.bat`
3. 対戦して動作確認

### 自分 vs 自分 でテスト

`Game.java` を一時的に変更：

```java
// 両方ともCOMにする
players[0] = new Com(0);
players[1] = new Com(1);
```

## 📚 参考資料

- [MonteCarloPlayer](ai/MonteCarloPlayer.md) - モンテカルロ法の詳細
- [QLearningPlayer](ai/QLearningPlayer.md) - Q学習の詳細
- [AI Overview](ai/AI-Overview.md) - AI全般の概要

---

[← Build and Run](Build-and-Run.md) | [File Structure →](File-Structure.md)

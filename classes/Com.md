# Com クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Com.java` |
| **役割** | コンピュータプレイヤーの実装 |
| **継承** | `Player` |
| **作者** | k_maeno |

## 📝 説明

重力四目並べのCom（コンピュータ）を表すクラス。`Player`クラスを継承して実装されています。

**⭐ 課題の変更対象ファイルです！**

## 🔧 コンストラクタ

### Com(String name, int order, String info, String imageURI)

```java
public Com(String name, int order, String info, String imageURI)
```

**パラメータ:**
- `name` - コンピュータのプレイヤー名
- `order` - 先攻後攻の順番
- `info` - プレイヤーの情報
- `imageURI` - プレイヤーの画像パス

### Com(int order)

```java
public Com(int order)
```

自作のComを確定させたいときに使用するコンストラクタ。

**パラメータ:**
- `order` - 先攻後攻の順番

**デフォルト値:**
- 名前: `"今　ピュウ太"`（グループ名を設定）
- 情報: `"MonteCarloAI"`（グループの説明）
- 画像: `"image/com_128x128.jpg"`

## 🔧 メソッド

### put(Judge judge)

```java
@Override
public int put(Judge judge) throws IOException
```

Com（コンピュータ）がコマを盤のどこにおくか（何列目か）を指定します。

**⭐ この部分を改造してAIを強化します！**

**パラメータ:**
- `judge` - 現在の盤面状況や、リーチ判定処理を取得するために使用

**戻り値:**
- `putColumn` - 列数（0～6）

## 📄 現在の実装

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int putColumn;

    //=======↓↓↓　TODO: 以下の部分のプログラムを改造してください。　↓↓↓======
    
    // Q-Learningが使えない場合はMonteCarloを使用
    MonteCarloPlayer mc = new MonteCarloPlayer(100);
    putColumn = mc.selectMove(grid, this.order);

    //=======↑↑↑　以上の部分にプログラミングしてください　↑↑↑======
    return putColumn;
}
```

## 🎯 利用可能な情報

### 盤面データ

```java
int[][] grid = judge.getGrid();
// grid[行][列]
// -1: 空（石なし）
//  0: 先手の石
//  1: 後手の石
```

### 自分のプレイヤー番号

```java
this.order  // 0: 先手, 1: 後手
```

### 合法手の確認

```java
judge.canPut(col)  // 置ける: col, 置けない: -1
```

## 💡 AI実装例

### 例1: モンテカルロ法

```java
MonteCarloPlayer mc = new MonteCarloPlayer(100);
putColumn = mc.selectMove(grid, this.order);
```

### 例2: Q学習

```java
QLearningPlayerV2 qlearner = new QLearningPlayerV2(6, 7);
qlearner.loadQ("qlearning_model.dat");
putColumn = qlearner.selectMove(grid, this.order, 0.01);
```

### 例3: ハイブリッド

```java
// 勝てる手があれば即座に打つ
int winMove = findWinningMove(grid, this.order);
if (winMove != -1) {
    putColumn = winMove;
} else {
    // それ以外はモンテカルロ
    MonteCarloPlayer mc = new MonteCarloPlayer(100);
    putColumn = mc.selectMove(grid, this.order);
}
```

## 📊 カスタマイズポイント

### 1. プレイヤー名の変更

```java
public Com(int order) {
    this(
        "チームA",  // ← グループ名に変更
        order,
        "最強AIを目指して",  // ← 説明を変更
        "image/team_a.jpg"  // ← 画像を変更
    );
}
```

### 2. AIロジックの改良

```java
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int putColumn;

    // ここを改造
    // - ルールベースAI
    // - 評価関数によるAI
    // - モンテカルロ法
    // - Q学習
    // - ミニマックス法
    // など

    return putColumn;
}
```

## 🔗 関連クラス

- [Player](Player.md) - 親クラス
- [Man](Man.md) - 対となる人間プレイヤー
- [MonteCarloPlayer](../ai/MonteCarloPlayer.md) - モンテカルロAI
- [QLearningPlayerV2](../ai/QLearningPlayerV2.md) - Q学習AI
- [Creating Custom AI](../Creating-Custom-AI.md) - AI作成ガイド

---

[← Man](Man.md) | [ImageButton →](ImageButton.md)

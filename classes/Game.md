# Game クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Game.java` |
| **役割** | ゲームの進行管理 |
| **作者** | k_maeno |

## 📝 説明

重力四目並べのゲームをコントロールするクラス。ゲームの状況、勝利条件などを管理します。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `protected final` | `int` | `MAX` | ゲームの最大石設置回数（行×列） |
| `protected` | `Recoder` | `recoder` | 棋譜記録用 |
| `protected` | `Board` | `board` | 盤面管理 |
| `protected` | `Judge` | `judge` | 審判管理 |
| `protected` | `Player[]` | `players` | プレイヤー配列（2人） |
| `protected` | `int` | `order` | 先手(0)/後手(1) |
| `protected` | `int` | `count` | 石設置回数 |
| `protected` | `boolean` | `nowPlaying` | ゲーム進行中フラグ |

## 🔧 コンストラクタ

### Game(int row, int col, int toWin)

```java
public Game(int row, int col, int toWin)
```

**パラメータ:**
- `row` - 盤面の行数
- `col` - 盤面の列数
- `toWin` - 勝つために必要な連続石数

**処理内容:**
1. 各フィールドを初期化
2. `Board` と `Judge` を生成
3. `init()` を呼び出してプレイヤーを設定

## 🔧 メソッド

### init()

```java
public void init()
```

プレイ状況の初期化。先攻後攻の設定もここで行います。

**処理内容:**
1. 先手・後手をランダムに決定
2. プレイヤーを生成（Man と Com）
3. プレイヤー情報を表示
4. 棋譜を初期化

### play()

```java
public void play()
```

ゲームの進行管理。勝利条件に達するまでループします。

**処理フロー:**

```
1. nowPlaying = true
2. Board を表示

ゲームループ開始:
├── 現在のプレイヤーを取得
├── put() で手を取得（合法手になるまで繰り返し）
├── Judge.put() で石を配置
├── Board.draw() で表示更新
├── Recoder.recode() で記録
├── メッセージ表示
├── Judge.checkWinner() で勝敗判定
│   └── 勝利なら nowPlaying = false
└── count++

ループ終了後:
├── 引き分け判定
├── 結果表示
├── 棋譜保存
└── Board.dispose()
```

## 📄 主要コード

```java
public void play() {
    nowPlaying = true;
    int putColumn = -1;
    String message = "";

    board.setVisible(true);

    Player currentPlayer;
    int row = -1;

    while(nowPlaying && count < MAX) {
        currentPlayer = players[count % 2];
        do {
            try {
                putColumn = currentPlayer.put(judge);
            } catch (IOException e) {
                e.printStackTrace();
            }
        } while(judge.canPut(putColumn) == -1);

        try {
            row = judge.put(currentPlayer.getOrder(), putColumn);
            judge.showGrid();
            board.draw(currentPlayer.getOrder(), row, putColumn);
        } catch (MalformedURLException | InterruptedException e) {
            e.printStackTrace();
        }

        recoder.recode(currentPlayer.getName(), currentPlayer.getStone(), putColumn);

        message = currentPlayer.getName() + "さんは、" + 
                  currentPlayer.getStone() + "を" + (putColumn + 1) + "に置きます。";
        board.dispKifu(message);
        System.out.println(message);

        if (judge.checkWinner(currentPlayer.order, row, putColumn)) {
            message = currentPlayer.getName() + "さんの勝ちです。";
            nowPlaying = false;
        }

        count++;
    }

    if(nowPlaying) {
        message = "この試合は、引き分けです。";
    }
    
    board.showResult(message);
    if(recoder.log()) {
        System.out.println("棋譜を保存しました。");
    } else {
        System.err.println("棋譜の保存に失敗しました。");
    }

    board.dispose();
}
```

## 🔗 関連クラス

- [Main](Main.md) - エントリーポイント
- [Board](Board.md) - 盤面UI
- [Judge](Judge.md) - 審判
- [Player](Player.md) - プレイヤー抽象クラス
- [Recoder](../tools/Recoder.md) - 棋譜記録

## 💡 カスタマイズ

### 対戦モードの変更

`init()` メソッドで対戦モードを変更できます：

```java
// 人間 vs COM（標準）
players[order] = new Man(order, board);
players[order2] = new Com(order2);

// COM vs COM
players[order] = new Com(order);
players[order2] = new Com(order2);

// 人間 vs 人間
players[order] = new Man(order, board);
players[order2] = new Man(order2, board);
```

---

[← Main](Main.md) | [Board →](Board.md)

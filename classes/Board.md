# Board クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Board.java` |
| **役割** | 盤面のUI表示 |
| **継承** | `JFrame` |
| **実装** | `ActionListener` |
| **作者** | k_maeno |

## 📝 説明

盤を表すクラス（背景画像を分割して表示する）。JFrameを継承して作成されています。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private` | `int` | `row` | 盤の行数 |
| `private` | `int` | `col` | 盤の列数 |
| `private` | `JLabel[][]` | `grid` | コマを表示するためのパネル |
| `private static` | `JButton[]` | `buttons` | 配置場所を決めるためのボタン |
| `private` | `int` | `pushButtonNo` | クリックされたマスの番号 |
| `private` | `int` | `counter` | 手の数 |
| `private` | `String` | `fileName` | 画像のファイル名用 |
| `private static` | `BottomScrollPanel` | `bottomScrollPanel` | 棋譜表示用 |
| `private` | `PlayerPanel` | `leftPanel` | 左プレイヤー情報 |
| `private` | `PlayerPanel` | `rightPanel` | 右プレイヤー情報 |

## 🔧 コンストラクタ

### Board(int x, int y)

```java
public Board(int x, int y)
```

**パラメータ:**
- `x` - 行数
- `y` - 列数

**処理内容:**
1. ウィンドウ設定
2. GridLayoutで盤面を構築
3. 各マスに画像を配置
4. ボタンを配置
5. サイドパネルと下部パネルを配置
6. 開始確認ダイアログを表示

## 🔧 メソッド

### dispKifu(String message)

```java
public void dispKifu(String message)
```

棋譜を表示します。

**パラメータ:**
- `message` - 表示するメッセージ

### draw(int order, int i, int j)

```java
public void draw(int order, int i, int j) throws MalformedURLException, InterruptedException
```

駒をボードの指定位置に置きます。石が落ちるアニメーションと効果音付きです。

**パラメータ:**
- `order` - 先手・後手の情報（0:先手/赤, 1:後手/青）
- `i` - 行番号
- `j` - 列番号

### resetDraw()

```java
public void resetDraw()
```

駒を初期状態にリセットします。

### actionPerformed(ActionEvent event)

```java
public void actionPerformed(ActionEvent event)
```

ボタンが押されたときのイベントハンドラ。

### resetPushButtonNo()

```java
public void resetPushButtonNo()
```

押されたボタン番号を初期化（-1にリセット）。

### getPushButtonNo()

```java
public int getPushButtonNo()
```

押されたボタンの列番号を返します。

**戻り値:**
- 押されたボタンの列番号（-1は未押下）

### showResult(String message)

```java
public void showResult(String message)
```

対戦結果を表示します。

### showPlayersData(Player[] players)

```java
public void showPlayersData(Player[] players)
```

両プレイヤーの情報をサイドパネルに表示します。

## 🎨 UI構成

```
┌────────────────────────────────────────────────────────────────┐
│ BorderLayout                                                    │
├──────────┬────────────────────────────────────┬──────────┤
│   WEST   │              CENTER                 │   EAST   │
│          │  ┌───────────────────────────────┐   │          │
│ Player   │  │    GridLayout (row+1 × col)    │   │ Player   │
│ Panel    │  │                               │   │ Panel    │
│          │  │  [JLabel] [JLabel] ... ×col   │   │          │
│          │  │  [JLabel] [JLabel] ... ×col   │   │          │
│          │  │  ...           ×row 行        │   │          │
│          │  │                               │   │          │
│          │  │  [Button] [Button] ... ×col   │   │          │
│          │  └───────────────────────────────┘   │          │
├──────────┴────────────────────────────────────┴──────────┤
│                        SOUTH                                │
│                 BottomScrollPanel                           │
└────────────────────────────────────────────────────────────────┘
```

## 📄 主要コード（コンストラクタ抜粋）

```java
public Board(int x, int y) {
    super("重力四目並べ（確認用）");
    row = x;
    col = y;
    counter = 1;
    resetPushButtonNo();

    grid = new JLabel[row][col];
    buttons = new JButton[col];
    setDefaultCloseOperation(EXIT_ON_CLOSE);

    Container container = getContentPane();
    container.setLayout(new BorderLayout());

    JPanel panel = new JPanel();
    GridLayout layout = new GridLayout(row + 1, col);
    panel.setPreferredSize(new Dimension(700, 600));
    panel.setLayout(layout);

    // 盤面グリッドの設定
    for(int i=0; i < row; i++) {
        for(int j=0; j < col;j++) {
            fileName = "image/kabe00.jpg";
            JLabel image = new JLabel(new ImageIcon(fileName));
            grid[i][j] = image;
            panel.add(image);
        }
    }

    // ボタンの設定
    for (int i = 0; i < col; i++) {
        fileName = "image/num0" + (i + 1) + ".jpg";
        ImageButton button = new ImageButton(i, fileName);
        button.setPreferredSize(new Dimension(100, 100));
        button.addActionListener(this);
        buttons[i] = button;
        panel.add(button);
    }

    // パネル配置
    bottomScrollPanel = new BottomScrollPanel();
    container.add(bottomScrollPanel, BorderLayout.SOUTH);

    leftPanel = new PlayerPanel();
    container.add(leftPanel, BorderLayout.WEST);

    rightPanel = new PlayerPanel();
    container.add(rightPanel, BorderLayout.EAST);

    container.add(panel, BorderLayout.CENTER);

    pack();
    setResizable(false);

    // 開始確認ダイアログ
    int result = JOptionPane.showConfirmDialog(this, 
        "対戦を開始しますか？", "重力4目並べ", JOptionPane.YES_NO_OPTION);
    if(result == 1) {
        System.exit(0);
    }
}
```

## 🔗 関連クラス

- [Game](Game.md) - ゲーム進行管理
- [ImageButton](ImageButton.md) - 画像ボタン
- [PlayerPanel](PlayerPanel.md) - プレイヤー情報パネル
- [BottomScrollPanel](BottomScrollPanel.md) - 棋譜表示パネル
- [Sound](Sound.md) - 効果音

---

[← Game](Game.md) | [Judge →](Judge.md)

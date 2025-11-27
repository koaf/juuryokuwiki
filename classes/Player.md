# Player クラス（抽象クラス）

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Player.java` |
| **役割** | プレイヤーの抽象基底クラス |
| **種別** | 抽象クラス |
| **サブクラス** | `Man`, `Com` |
| **作者** | k_maeno |

## 📝 説明

プレイヤー抽象クラス。`Man`（人間）と`Com`（コンピュータ）に継承されます。

## 🔧 定数

| 修飾子 | 型 | 名前 | 値 | 説明 |
|--------|------|------|------|------|
| `public static final` | `String` | `DEFAULT_NAME` | `"名無しさん"` | デフォルト名 |
| `public static final` | `String` | `DEFAULT_INFO` | `"プレイヤー概要"` | デフォルト説明 |
| `public static final` | `String` | `DEFAULT_IMAGE_URI` | `"image/player_128x128.jpg"` | デフォルト画像 |

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private` | `String` | `name` | プレイヤーの名前 |
| `protected` | `int` | `order` | 先手・後手（0:先手, 1:後手） |
| `protected` | `String` | `message` | プレイヤーの情報 |
| `protected` | `ImageIcon` | `imageIcon` | プレイヤーの画像 |

## 🔧 コンストラクタ

### Player(String name, int order, String info, String imageURI)

```java
public Player(String name, int order, String info, String imageURI)
```

**パラメータ:**
- `name` - プレイヤー名
- `order` - 先攻後攻
- `info` - プレイヤーの情報
- `imageURI` - プレイヤーの画像パス

**処理内容:**
- フィールドを初期化
- 画像ファイルが存在しない場合はデフォルト画像を使用

## 🔧 抽象メソッド

### put(Judge judge)

```java
public abstract int put(Judge judge) throws IOException;
```

コマを置く場所を決めるための抽象メソッド。`Man`と`Com`で挙動が異なるため、継承先で具体的な動作を定義します。

**パラメータ:**
- `judge` - 戦況を判断するための審判クラスのインスタンス

**戻り値:**
- 列の番号（0～6）

**例外:**
- `IOException` - データ保存時に発生（現在は別クラスで実装）

## 🔧 ゲッターメソッド

### getName()

```java
public String getName()
```

名前を返します。

### getOrder()

```java
public int getOrder()
```

先手・後手のどちらかを返します。

**戻り値:**
- `0`: 先手
- `1`: 後手

### getImageIcon()

```java
public ImageIcon getImageIcon()
```

プレイヤーの画像を返します。

### getMessage()

```java
public String getMessage()
```

プレイヤーのメッセージ（概要）を返します。

### getStone()

```java
public String getStone()
```

先手・後手に応じた石の形状を返します。

**戻り値:**
- `"○"`: 先手の場合
- `"×"`: 後手の場合

## 📊 クラス階層

```
              Player (abstract)
                    │
          ┌─────────┴─────────┐
          │                   │
         Man                 Com
    (人間プレイヤー)      (コンピュータ)
```

## 📄 ソースコード

```java
public abstract class Player {
    public static final String DEFAULT_NAME = "名無しさん";
    public static final String DEFAULT_INFO = "プレイヤー概要";
    public static final String DEFAULT_IMAGE_URI = "image/player_128x128.jpg";

    private String name;
    protected int order;
    protected String message;
    protected ImageIcon imageIcon;

    public Player(String name, int order, String info, String imageURI) {
        this.name = name;
        this.order = order;
        this.message = info;

        Path path = Paths.get(imageURI);
        if(Files.exists(path)) {
            imageIcon = new ImageIcon(imageURI);
        } else {
            imageIcon = new ImageIcon(DEFAULT_IMAGE_URI);
        }
    }

    public abstract int put(Judge judge) throws IOException;

    public String getName() { return this.name; }
    public int getOrder() { return this.order; }
    public ImageIcon getImageIcon() { return this.imageIcon; }
    public String getMessage() { return this.message; }
    
    public String getStone() {
        return (this.order == 0) ? "○" : "×";
    }
}
```

## 🔗 関連クラス

- [Man](Man.md) - 人間プレイヤー（Playerを継承）
- [Com](Com.md) - コンピュータプレイヤー（Playerを継承）
- [PlayerPanel](PlayerPanel.md) - プレイヤー情報表示

---

[← Judge](Judge.md) | [Man →](Man.md)

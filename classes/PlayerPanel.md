# PlayerPanel クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `PlayerPanel.java` |
| **役割** | プレイヤー情報表示パネル |
| **継承** | `JPanel` |
| **作者** | k_maeno |

## 📝 説明

プレイヤー情報を表示するサイドパネルのクラスです。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private` | `JLabel` | `imageLabel` | プレイヤーの画像を表示 |
| `private` | `JLabel` | `nameLabel` | プレイヤーの名前を表示 |
| `private` | `JTextArea` | `infoArea` | プレイヤーの概要を表示 |

## 🔧 コンストラクタ

### PlayerPanel()

```java
public PlayerPanel()
```

デフォルトコンストラクタ。`Player`クラスのデフォルト情報を使用します。

### PlayerPanel(String imageURI, String playerName, String info)

```java
public PlayerPanel(String imageURI, String playerName, String info)
```

**パラメータ:**
- `imageURI` - 画像ファイルのパス文字列
- `playerName` - プレイヤーの名前
- `info` - プレイヤーの概要などの情報

**処理内容:**
1. `BoxLayout`（縦方向）を設定
2. 画像ラベルを追加
3. 名前ラベルを追加
4. 概要テキストエリアを追加

## 🔧 メソッド

### setData(Player player)

```java
public void setData(Player player)
```

サイドパネルに指定したプレイヤーの情報を反映させます。

**パラメータ:**
- `player` - 表示するプレイヤー

**処理内容:**
1. 画像を切り替え
2. 名前を切り替え
3. 概要情報を切り替え

## 🎨 レイアウト

```
┌─────────────────┐
│                 │
│   ┌─────────┐   │
│   │  画像    │   │ ← imageLabel
│   │ 128x128 │   │
│   └─────────┘   │
│                 │
│   プレイヤー名   │ ← nameLabel
│                 │
│   概要テキスト   │ ← infoArea
│   (複数行)      │
│   ...           │
│                 │
└─────────────────┘
```

## 📄 ソースコード

```java
public class PlayerPanel extends JPanel {
    private JLabel imageLabel;
    private JLabel nameLabel;
    private JTextArea infoArea;

    public PlayerPanel() {
        this(Player.DEFAULT_IMAGE_URI, Player.DEFAULT_NAME, Player.DEFAULT_INFO);
    }

    public PlayerPanel(String imageURI, String playerName, String info) {
        setLayout(new BoxLayout(this, BoxLayout.Y_AXIS));

        Path path = Paths.get(imageURI);
        ImageIcon imageIcon = (Files.exists(path)) 
            ? new ImageIcon(imageURI) 
            : new ImageIcon(Player.DEFAULT_IMAGE_URI);
        imageLabel = new JLabel(imageIcon);
        add(imageLabel);
        add(Box.createVerticalStrut(5));

        nameLabel = new JLabel(playerName);
        nameLabel.setFont(new Font("ＭＳ ゴシック", Font.PLAIN, 12));
        add(nameLabel);
        add(Box.createVerticalStrut(5));

        infoArea = new JTextArea(10, 12);
        infoArea.setEditable(false);
        infoArea.setFocusable(false);
        infoArea.setLineWrap(true);
        infoArea.setWrapStyleWord(true);
        infoArea.setOpaque(false);
        infoArea.setFont(new Font("ＭＳ ゴシック", Font.PLAIN, 12));
        infoArea.setText(info);

        infoArea.setSize(infoArea.getPreferredSize().width, Short.MAX_VALUE);
        Dimension preferred = infoArea.getPreferredSize();
        infoArea.setPreferredSize(new Dimension(infoArea.getWidth(), preferred.height));

        add(infoArea);
    }

    public void setData(Player player) {
        ImageIcon imageIcon = player.getImageIcon();
        imageIcon.getImage().flush();
        imageLabel.setIcon(imageIcon);

        nameLabel.setText(player.getName());
        infoArea.setText(player.getMessage());
    }
}
```

## 🔗 関連クラス

- [Board](Board.md) - PlayerPanelを左右に配置
- [Player](Player.md) - プレイヤー情報の取得元

---

[← ImageButton](ImageButton.md) | [BottomScrollPanel →](BottomScrollPanel.md)

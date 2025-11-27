# BottomScrollPanel クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `BottomScrollPanel.java` |
| **役割** | 棋譜表示パネル |
| **継承** | `JScrollPane` |
| **作者** | k_maeno |

## 📝 説明

ボトムパネル（棋譜）を表すクラス。`JScrollPane`を継承して実装されています。自動スクロール機能付きです。

## 🔧 定数

| 修飾子 | 型 | 名前 | 値 | 説明 |
|--------|------|------|------|------|
| `private static final` | `int` | `BOTTOM_WIDTH` | `760` | パネル幅 |
| `private static final` | `int` | `BOTTOM_HEIGHT` | `48` | パネル高さ |

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private static` | `ScrollTextArea` | `text` | インナークラスのインスタンス |

## 🔧 コンストラクタ

### BottomScrollPanel()

```java
public BottomScrollPanel()
```

**処理内容:**
1. 親クラスに`ScrollTextArea`を渡す
2. サイズを設定（760×48）

## 🔧 メソッド

### append(String str)

```java
public void append(String str)
```

文字列を追加して表示します。自動的に最下部にスクロールします。

**パラメータ:**
- `str` - 表示したい文字列

## 📦 インナークラス

### ScrollTextArea

```java
private static class ScrollTextArea extends JTextArea
```

自動スクロールするテキストエリアをインナークラスとして実装。

#### コンストラクタ

```java
private ScrollTextArea()
```

テキスト領域の行折り返しを有効化。

#### append(String arg0)

```java
public void append(String arg0)
```

棋譜に表示する文章の追加と共に自動で最下部までスクロールします。

**パラメータ:**
- `arg0` - 棋譜に記載するテキスト

**処理内容:**
1. 親クラスの`append()`で文字列追加（改行付き）
2. 可視領域を下にシフト
3. 自動スクロール

## 📄 ソースコード

```java
public class BottomScrollPanel extends JScrollPane {
    private static ScrollTextArea text = new ScrollTextArea();
    private static final int BOTTOM_WIDTH = 760;
    private static final int BOTTOM_HEIGHT = 48;

    public BottomScrollPanel() {
        super(text);
        this.setPreferredSize(new Dimension(BOTTOM_WIDTH, BOTTOM_HEIGHT));
    }

    public void append(String str) {
        this.text.append(str);
    }

    private static class ScrollTextArea extends JTextArea {
        private ScrollTextArea() {
            this.setLineWrap(true);
        }

        public void append(String arg0) {
            super.append(arg0 + "\n");
            int h = this.getRowHeight();
            Rectangle rect = this.getVisibleRect();
            rect.y += h;
            this.scrollRectToVisible(rect);
        }
    }
}
```

## 🎨 表示例

```
┌────────────────────────────────────────────────────────────────┐
│ 人間さんは、○を4に置きます。                                   │
│ 今　ピュウ太さんは、×を4に置きます。                           │
│ 人間さんは、○を3に置きます。                                   │ ← 自動スクロール
└────────────────────────────────────────────────────────────────┘
```

## 🔗 関連クラス

- [Board](Board.md) - BottomScrollPanelを下部に配置

---

[← PlayerPanel](PlayerPanel.md) | [Sound →](Sound.md)

# ImageButton クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `ImageButton.java` |
| **役割** | 画像付きボタン |
| **継承** | `JButton` |
| **作者** | k_maeno |

## 📝 説明

画像を簡単に貼り付けるようにして、何列目のボタンかを使いやすくするために`JButton`を継承して作成されています。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private` | `int` | `index` | 何列目のボタンか |

## 🔧 コンストラクタ

### ImageButton(Integer index, String imageURI)

```java
public ImageButton(Integer index, String imageURI)
```

**パラメータ:**
- `index` - 何列目か（0～6）
- `imageURI` - 画像のファイルパス

**処理内容:**
1. 親クラス`JButton`に画像を渡す
2. `index`を保存
3. 画像ファイルが無い場合は列数を文字として表示

## 🔧 メソッド

### getIndex()

```java
public int getIndex()
```

押されたボタンが何列目かを返します。

**戻り値:**
- 保持する列番号（0～6）

## 📄 ソースコード

```java
public class ImageButton extends JButton {
    private int index;

    public ImageButton(Integer index, String imageURI) {
        super(new ImageIcon(imageURI));
        this.index = index;

        Path path = Paths.get(imageURI);
        if(!Files.exists(path)) {
            // ファイルがない場合の処理
            setFont(new Font("Consolas", Font.PLAIN, 36));
            setText(Integer.toString(index));
        }
    }

    public int getIndex() {
        return this.index;
    }
}
```

## 🎨 使用例

### Board.java での使用

```java
// ボタンの作成
for (int i = 0; i < col; i++) {
    fileName = "image/num0" + (i + 1) + ".jpg";
    ImageButton button = new ImageButton(i, fileName);
    button.setPreferredSize(new Dimension(100, 100));
    button.addActionListener(this);
    buttons[i] = button;
    panel.add(button);
}
```

### イベントハンドラでの使用

```java
public void actionPerformed(ActionEvent event) {
    if(event.getSource() instanceof ImageButton) {
        pushButtonNo = ((ImageButton)event.getSource()).getIndex();
    }
}
```

## 🔗 関連クラス

- [Board](Board.md) - ImageButtonを使用

---

[← Com](Com.md) | [PlayerPanel →](PlayerPanel.md)

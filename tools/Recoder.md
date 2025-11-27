# Recoder クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Recoder.java` |
| **役割** | 棋譜の記録と保存 |
| **作者** | k_maeno |

## 📝 説明

棋譜を残すためのクラス。今後の利用用途としては、指定した譜面の再生（Comクラス同士の対戦として実装可能）があります。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private final` | `String` | `lineSeparator` | OS毎の改行コード |
| `private` | `String` | `gameRecode` | 記録する譜面の情報 |

## 🔧 メソッド

### init(Player[] players)

```java
public void init(Player[] players)
```

誰と誰の対局かを`gameRecode`に記録します。

**パラメータ:**
- `players` - 対戦するプレイヤー2名の情報

**記録形式:**
```
プレイヤー1名<%%vs%%>プレイヤー2名
```

### recode(String player, String mark, int col)

```java
public void recode(String player, String mark, int col)
```

各指し手を1行で`gameRecode`に記録します。

**パラメータ:**
- `player` - プレイヤー名
- `mark` - 石の形状（○ or ×）
- `col` - 何列目に設置したか

**記録形式:**
```
プレイヤー名<%%/%%>石<%%/%%>列番号
```

### log()

```java
public boolean log()
```

譜面を`log`フォルダに保存します。ファイル名は現在の日付・時刻です。

**戻り値:**
- `true`: 保存成功
- `false`: 保存失敗

**ファイル名形式:** `yyyyMMdd_HHmmss.txt`

## 📄 ソースコード

```java
public class Recoder {
    private final String lineSeparator = System.getProperty("line.separator");
    private String gameRecode = "";

    public void init(Player[] players) {
        gameRecode = players[0].getName() + "<%%vs%%>" + 
                     players[1].getName() + lineSeparator;
    }

    public void recode(String player, String mark, int col) {
        gameRecode += player + "<%%/%%>" + mark + "<%%/%%>" + col + lineSeparator;
    }

    public boolean log() {
        LocalDateTime now = LocalDateTime.now();
        DateTimeFormatter formatter = DateTimeFormatter.ofPattern("yyyyMMdd_HHmmss");
        String formatNowDate = formatter.format(now);
        try {
            File file = new File(".\\log\\" + formatNowDate + ".txt");
            FileWriter fileWriter = new FileWriter(file);
            PrintWriter writer = new PrintWriter(new BufferedWriter(fileWriter));

            writer.println(this.gameRecode);
            writer.close();
        } catch (IOException e) {
            e.printStackTrace();
            return false;
        }

        return true;
    }
}
```

## 📁 出力ファイル例

### ファイル名
```
log/20251127_143025.txt
```

### ファイル内容
```
人間<%%vs%%>今　ピュウ太
人間<%%/%%>○<%%/%%>3
今　ピュウ太<%%/%%>×<%%/%%>4
人間<%%/%%>○<%%/%%>3
今　ピュウ太<%%/%%>×<%%/%%>2
人間<%%/%%>○<%%/%%>3
今　ピュウ太<%%/%%>×<%%/%%>5
人間<%%/%%>○<%%/%%>3
```

## 📊 デリミタの説明

| デリミタ | 用途 |
|----------|------|
| `<%%vs%%>` | プレイヤー名の区切り |
| `<%%/%%>` | 指し手情報の区切り |

## 💡 今後の拡張案

### 棋譜の再生

```java
// 棋譜ファイルを読み込んで再生
public void replay(String logFile) {
    // ファイル読み込み
    // デリミタで分割
    // 盤面を再現
}
```

### COM同士の対戦記録分析

```java
// 複数の棋譜から勝率を計算
// 特定の局面でのAIの選択を分析
```

## 🔗 関連クラス

- [Game](../classes/Game.md) - Recoderを使用して棋譜を記録
- [Player](../classes/Player.md) - プレイヤー情報の取得元

---

[← ModelOptimizer](ModelOptimizer.md) | [Home →](../Home.md)

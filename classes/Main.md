# Main クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Main.java` |
| **役割** | アプリケーションのエントリーポイント |
| **作者** | k_maeno |

## 📝 説明

ゲームを開始するためのクラスです。1試合が終わると次のゲームに移り、無限ループで対戦が続きます。

## 🔧 メソッド

### main(String[] args)

```java
public static void main(String[] args)
```

メインメソッド。ここからすべてのプログラムが動いていきます。

**パラメータ:**
- `args` - コマンドライン引数（今回は使用しない）

**処理内容:**
1. `Game` オブジェクトを生成（6行×7列×4目並べ）
2. `play()` メソッドでゲームを実行
3. ゲーム終了後、新しいゲームを開始（無限ループ）

## 📄 ソースコード

```java
/**
 * ゲームを開始するためのクラス。1試合が終わると次のゲームに移る
 * @author k_maeno
 */
public class Main {
    /**
     * メインメソッド。ここからすべてのプログラムが動いていく。
     * @param args 今回は使用しない（コマンドライン引数から情報を受ける場合に使用）
     */
    public static void main(String[] args) {
        while(true) {
            Game game = new Game(6, 7, 4);
            game.play();
        }
    }
}
```

## 🔗 関連クラス

- [Game](Game.md) - ゲーム進行を管理するクラス

## 💡 カスタマイズ

### ゲームパラメータの変更

```java
// 標準: 6行×7列×4目並べ
Game game = new Game(6, 7, 4);

// 例: 8行×8列×5目並べ
Game game = new Game(8, 8, 5);
```

### 1回だけ実行

```java
public static void main(String[] args) {
    Game game = new Game(6, 7, 4);
    game.play();
    // ループしないので1回で終了
}
```

---

[← Class Diagram](../Class-Diagram.md) | [Game →](Game.md)

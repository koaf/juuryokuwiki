# Getting Started - 始め方

## 🔧 セットアップ

### 必要環境

1. **Java Development Kit (JDK)**
   - バージョン: 8以上
   - 環境変数 `JAVA_HOME` と `PATH` が設定されていること

2. **IDE（推奨）**
   - Eclipse
   - IntelliJ IDEA
   - Visual Studio Code (Java Extension Pack)

### インストール確認

コマンドプロンプトで以下を実行して、Javaがインストールされているか確認してください：

```batch
java -version
javac -version
```

## 🚀 プロジェクトの実行

### 方法1: バッチファイルを使用（推奨）

1. **コンパイル**
   ```batch
   @compile.bat
   ```

2. **ゲーム実行**
   ```batch
   @start.bat
   ```

### 方法2: コマンドラインから直接

```batch
# コンパイル
javac Main.java

# 実行
java Main
```

### 方法3: IDEを使用

1. プロジェクトをIDEにインポート
2. `Main.java` を右クリック
3. 「実行」または「Run」を選択

## 📚 Javadocの生成

```batch
@javadoc.bat
```

生成されたドキュメントは `doc/` フォルダに保存されます。ブラウザで `doc/index.html` を開くと閲覧できます。

## 🎮 ゲームの開始

1. ゲームを起動すると、確認ダイアログが表示されます
2. 「はい」をクリックして対戦開始
3. 先手・後手はランダムに決定されます
4. 人間プレイヤーはボタンをクリックして石を置きます
5. 4つ連続で石を並べたら勝利！

## 📝 初めての変更

課題の主目的は `Com.java` を改良することです：

```java
// Com.java の put() メソッドを探してください
@Override
public int put(Judge judge) throws IOException {
    int[][] grid = judge.getGrid();
    int putColumn;

    //=======↓↓↓　TODO: 以下の部分のプログラムを改造してください。　↓↓↓======
    
    // ここにあなたのAIロジックを実装！
    
    //=======↑↑↑　以上の部分にプログラミングしてください　↑↑↑======
    return putColumn;
}
```

## 🔍 トラブルシューティング

### コンパイルエラーが出る場合

- JDKが正しくインストールされているか確認
- 文字コードがUTF-8になっているか確認
- すべての `.java` ファイルが同じフォルダにあるか確認

### ゲームが起動しない場合

- `image/` フォルダが存在するか確認
- `sound/` フォルダが存在するか確認
- 必要な画像・音声ファイルがあるか確認

### 音が鳴らない場合

- `sound/fall.wav` が存在するか確認
- システムの音量設定を確認

---

[← ホームに戻る](Home.md) | [ゲームルール →](Game-Rules.md)

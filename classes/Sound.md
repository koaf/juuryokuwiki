# Sound クラス

## 📋 基本情報

| 項目 | 内容 |
|------|------|
| **ファイル** | `Sound.java` |
| **役割** | 効果音の再生 |
| **作者** | k_maeno |

## 📝 説明

音を鳴らすためのクラス。Javaの`javax.sound.sampled`パッケージを使用しています。

## 🔧 フィールド

| 修飾子 | 型 | 名前 | 説明 |
|--------|------|------|------|
| `private` | `Clip` | `clip` | 音声クリップ |

## 🔧 コンストラクタ

### Sound(String file)

```java
public Sound(String file) throws MalformedURLException
```

**パラメータ:**
- `file` - サウンドファイル名（相対パス）

**例外:**
- `MalformedURLException` - パスに不正な文字列があった場合

**処理内容:**
1. ファイルパスを作成
2. `AudioInputStream`で音声ファイルを読み込み
3. `Clip`を取得してオープン
4. エラー時は`clip`を`null`に設定

## 🔧 メソッド

### play()

```java
public void play()
```

音源を再生します。

**処理内容:**
1. `clip`が`null`でなければ
2. 再生位置を先頭に戻す
3. 再生開始

### stop()

```java
public void stop()
```

音を停止します。

## 📄 ソースコード

```java
public class Sound {
    private Clip clip;

    public Sound(String file) throws MalformedURLException {
        String path = System.getProperty("user.dir") + "/" + file;
        try {
            AudioInputStream inputStream = AudioSystem.getAudioInputStream(new File(path));
            this.clip = AudioSystem.getClip();
            this.clip.open(inputStream);
        } catch (UnsupportedAudioFileException | IOException e) {
            e.printStackTrace();
            this.clip = null;
        } catch (LineUnavailableException e) {
            e.printStackTrace();
            this.clip = null;
        }
    }

    public void play() {
        if(clip != null) {
            clip.setMicrosecondPosition(0);
            clip.start();
        }
    }

    public void stop() {
        if(clip != null) {
            clip.stop();
        }
    }
}
```

## 🎵 使用されている音声ファイル

| ファイル | 用途 |
|----------|------|
| `sound/fall.wav` | 石が落ちる効果音 |

## 🔧 使用例

### Board.java での使用

```java
public void draw(int order, int i, int j) throws MalformedURLException, InterruptedException {
    Sound fall = new Sound("sound/fall.wav");
    fall.play();
    
    // アニメーション処理...
    
    fall.stop();
}
```

## ⚠️ 注意事項

- サポートされる音声形式: WAV, AIFF, AU など
- MP3は標準ではサポートされていません
- ファイルが見つからない場合、例外をキャッチして`clip`を`null`に設定
- `clip`が`null`の場合、`play()`と`stop()`は何もしない

## 🔗 関連クラス

- [Board](Board.md) - Soundを使用して効果音を再生

---

[← BottomScrollPanel](BottomScrollPanel.md) | [AI Overview →](../ai/AI-Overview.md)

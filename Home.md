# 重力四目並べ（Connect Four）プロジェクト Wiki

## 🎮 プロジェクト概要

このプロジェクトは、**重力四目並べ（Connect Four）** のJava実装です。プログラミング実習Ⅱのグループ課題として作成されました。人間プレイヤーとコンピュータ（AI）の対戦ができ、AI部分をカスタマイズすることが課題の主目的です。

## 📋 目次

### 基本情報
- [Home](Home.md) - このページ
- [Getting Started](Getting-Started.md) - 始め方・セットアップ
- [Game Rules](Game-Rules.md) - ゲームルール

### アーキテクチャ
- [Architecture Overview](Architecture-Overview.md) - システム全体の設計
- [Class Diagram](Class-Diagram.md) - クラス図と継承関係

### クラスリファレンス
- [Main](classes/Main.md) - エントリーポイント
- [Game](classes/Game.md) - ゲーム進行管理
- [Board](classes/Board.md) - 盤面UI
- [Judge](classes/Judge.md) - 審判・勝敗判定
- [Player](classes/Player.md) - プレイヤー抽象クラス
- [Man](classes/Man.md) - 人間プレイヤー
- [Com](classes/Com.md) - コンピュータプレイヤー

### UI コンポーネント
- [ImageButton](classes/ImageButton.md) - 画像ボタン
- [PlayerPanel](classes/PlayerPanel.md) - プレイヤー情報パネル
- [BottomScrollPanel](classes/BottomScrollPanel.md) - 棋譜表示パネル
- [Sound](classes/Sound.md) - 効果音

### AI アルゴリズム
- [AI Overview](ai/AI-Overview.md) - AI実装概要
- [MonteCarloPlayer](ai/MonteCarloPlayer.md) - モンテカルロ法AI
- [QLearningPlayer](ai/QLearningPlayer.md) - Q学習AI (v1)
- [QLearningPlayerV2](ai/QLearningPlayerV2.md) - Q学習AI (v2)
- [QLearningTrainer](ai/QLearningTrainer.md) - Q学習トレーナー

### ツール
- [ModelValidator](tools/ModelValidator.md) - モデル検証ツール
- [ModelOptimizer](tools/ModelOptimizer.md) - モデル最適化ツール
- [Recoder](tools/Recoder.md) - 棋譜記録

### 開発ガイド
- [Build and Run](Build-and-Run.md) - ビルドと実行方法
- [Creating Custom AI](Creating-Custom-AI.md) - オリジナルAIの作成方法
- [File Structure](File-Structure.md) - ファイル構成

---

## 🚀 クイックスタート

```batch
# コンパイル
@compile.bat

# ゲーム実行
@start.bat

# Javadoc生成
@javadoc.bat
```

## 📦 システム要件

- **Java**: JDK 8以上
- **OS**: Windows（バッチファイル使用）

## 👥 開発者向け情報

課題の主目的は `Com.java` の `put()` メソッドを改良して、より強いAIを作成することです。

---

*このWikiは自動生成されました*

# 作品名: シンカイサンタ

![C++](https://img.shields.io/badge/Language-C%2B%2B-00599C?style=flat-square)
![Framework](https://img.shields.io/badge/GameEngine-Siv3D-E34F26?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

シンプルで簡単操作の縦スクロール2Dアクションゲームです．
拡張性と保守性を意識して，C++とSiv3Dで制作しました．

## 作品概要
* ジャンル: 縦スクロール2Dアクション
* 開発環境: Visual Studio 2022 / C++23
* ゲームエンジン: Siv3D v0.6.16
* 制作期間: 約1ヶ月 (2025/10/18~)

## 開発メンバーと担当箇所
* [Kanta Muramatsu](https://github.com/kmqtm)(**私**): 企画，プログラム設計・実装
* [pipipimochi](https://scrapbox.io/bnscup2025/pipipimochi): 一部のプログラム(※)，グラフィック，サウンド

(※) [`GameScene.cpp`](src/Scene/GameScene.cpp)の360-418行のラムダ式および同ファイル420-450行の背景オブジェクトの配置に当たるプログラム部分

## 開発の概要と生成AIの活用方法
本作品は下記のゲームジャムイベントで制作されたゲームです．
2週間の開催期間の後に，追加で2週間のブラッシュアップ期間を設けました．
プログラムの設計は私が主導的に行い，生成AIを用いて実装し，私が適宜修正する形で開発を進めました．

* イベント名: [バンダイナムコスタジオ杯Siv3Dゲームジャム](https://siv3d.github.io/ja-jp/event/gamejam/)
* 開催期間: 2025/10/18(土)~2025/11/02(日)
* イベント内でのゲーム紹介ページ: [シンカイサンタ(Cosense)](https://scrapbox.io/bnscup2025/%E3%82%B7%E3%83%B3%E3%82%AB%E3%82%A4%E3%82%B5%E3%83%B3%E3%82%BF)

生成AIについては，開発初期にWeb上のGPT-5,Gemini 2.5 Proを用いて私が考えたプログラムの設計方針等について評価・改善しました．
コーディング時にはVisual StudioのIDE内でCopilotを使用しました．
Copilotで用いるモデルは，実装・修正にGPT-5を，Doxygen生成用のコメント作成等にはClaude Haiku 4.5を使用しました．
開発場面に合わせたモデル選択を行うことで，短期間で効率的に開発を進めることができました．

## 技術的なこだわり
本作品の制作にあたって注力した設計上の工夫を一部紹介します．

### 1. Strategyパターンを用いた敵の行動設計
敵キャラクターの行動ロジックを `IBehaviorStrategy` インターフェースとして分離して実装しました．
`Enemy` クラス本体を変更することなく，新しい行動パターンを容易に追加できる設計となっています．

* Code: [`src/AI/IBehaviorStrategy.h`](src/AI/IBehaviorStrategy.h), [`src/AI/BehaviorStrategies.cpp`](src/AI/BehaviorStrategies.cpp)

### 2. JSONによるデータ駆動
敵キャラクターのパラメータ(速度，物理サイズ，アニメーション設定など)をJSONファイルに分離しました．
コンパイル不要でゲームバランスの調整が可能となっています．

* Code: [`src/System/EnemyDataSystem.cpp`](src/System/EnemyDataSystem.cpp)
* Data: [`App/asset/EnemyData.json`](App/asset/EnemyData.json)

### 3. 責務の分離
「単一責任の原則」を意識し，機能ごとにクラスが独立するように設計しました．
GameSceneが肥大化するのを防ぎ，各システムの結合度を下げています．
また，分離後の各クラスには依然として改良の余地が認められるため，今後の課題として認識しています．

### 4. その他の工夫点(概要)
* ComponentとEntityの分離: ゲーム内のオブジェクト([`Entity`](src/Entity))が持ちうる要素を[`Component`](src/Component)として独立させ，複数のオブジェクトで再利用可能にしました．
* Stage作成の効率化: [`Tiled`](https://www.mapeditor.org/)というマップエディタを用いてステージを編集し，Exportしたステージデータをプログラムで読み取れるようにしました．これによって，ステージのタイル絵や衝突判定，オブジェクトの配置等を効率化できました．

## 📂 ディレクトリ構成 (主要ファイルのみ抜粋)
```text
Root
├── AI/                 # Strategyパターンによる敵の行動ロジック
│   ├── IBehaviorStrategy.h
│   └── BehaviorStrategies.cpp
├── Component/          # オブジェクトに持たせる要素
│   ├── Collider.h      # 衝突判定用の構造体等
│   └── AnimationController.cpp
├── Entitie/            # ゲーム内のオブジェクト
│   ├── Player.cpp      # 物理/状態等の管理
│   └── Enemy.cpp
├── System/             # 単一で動作するシステム群
│   ├── AssetSystem.cpp # リソース管理用
│   ├── CollisionSystem.cpp
│   └── EnemyDataSystem.cpp # 敵データ(JSON)の管理
├── World/              # ステージ管理
│   └── Stage.cpp       # Tiledマップの読み込み等
├── Scene/              # シーン管理
│   └── GameScene.cpp   # ゲームシーンの定義と更新
└── Main.cpp            # メインループ
```
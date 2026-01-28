# 作品名: シンカイサンタ

![C++](https://img.shields.io/badge/Language-C%2B%2B-00599C?style=flat-square)
![Framework](https://img.shields.io/badge/GameEngine-Siv3D-E34F26?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

* シンプルで簡単操作の縦スクロール2Dアクションゲームです．
* 拡張性と保守性を意識して，C++とSiv3Dで制作しました．

## 作品概要
* ジャンル: 縦スクロール2Dアクション
* 開発環境: Visual Studio 2022 / C++23
* ゲームエンジン: Siv3D v0.6.16
* 制作期間: 約1ヶ月 (2025/10/18~)

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
`GameScene` が肥大化するのを防ぎ，各システムの結合度を下げています．
分離後の各クラスは改良の余地が認められますが，基本的な設計方針として有効であると考えています．

### 4. その他の工夫点(概要)
* ComponentとEntityの分離: ゲーム内のオブジェクト([`Entity`](src/Entity))が持ちうる要素を[`Component`](src/Component)として独立させ，複数のオブジェクトで再利用可能にしました．
* Stageに配置するオブジェクトの自動配置等: [`Tiled`](https://www.mapeditor.org/)というマップエディタを用いてステージを設計し，Exportしたファイルを読み取るようにしました．これによって，ステージのタイル絵や衝突判定，オブジェクトの配置等を自動化できました．

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
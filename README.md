# 作品名: シンカイサンタ

![C++](https://img.shields.io/badge/Language-C%2B%2B20-00599C?style=flat-square)
![Framework](https://img.shields.io/badge/GameEngine-Siv3D-E34F26?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

**拡張性と保守性に優れる2D深海探索アクションゲーム．**
C++とSiv3D(ゲームエンジン)でアーキテクチャを構築しました．



## プロジェクト概要
* **ジャンル**: 2D探索型アクション(深海探索・酸素管理)
* **開発環境**: Visual Studio 2022 / C++20
* **ゲームエンジン**: Siv3D
* **制作期間**: 1ヶ月 (2025/10/18~)

## 技術的なこだわり (Technical Highlights)
本プロジェクトで特に注力した設計ポイントを紹介します．

### 1. StrategyパターンによるAI設計
敵キャラクターの行動ロジックを `IBehaviorStrategy` インターフェースとして分離．
`Enemy` クラス本体を変更することなく，新しい行動パターン(巡回，待機，追跡など)を容易に追加できる設計となっています．

* **Code:** [`src/AI/IBehaviorStrategy.h`](src/AI/IBehaviorStrategy.h), [`src/AI/BehaviorStrategies.cpp`](src/AI/BehaviorStrategies.cpp)

### 2. JSONによるデータ駆動 (Data-Driven)
キャラクターのパラメータ(速度，物理サイズ，アニメーション設定など)をJSONファイルに分離．
コンパイル不要でゲームバランスの調整が可能となっています．

* **Code:** [`src/System/EnemyDataSystem.cpp`](src/System/EnemyDataSystem.cpp)
* **Data:** [`App/asset/EnemyData.json`](App/asset/EnemyData.json)

### 3. 責務の分離とシステムアーキテクチャ
「単一責任の原則」に基づき，機能ごとに管理クラスを独立させています．
`GameScene` が巨大化するのを防ぎ，各システムの結合度を下げています．

* **CollisionSystem**: エンティティ間の衝突判定管理(O(N)の回避とフィルタリング)
* **CameraSystem**: プレイヤー追従と座標変換ロジック
* **UISystem**: 深度やステータスに応じた描画管理

## 📂 ディレクトリ構成 (主要ファイルのみ抜粋)
```text
Root
├── AI/                 # Strategyパターンによる敵AIロジック
│   ├── IBehaviorStrategy.h
│   └── BehaviorStrategies.cpp
├── Component/          # ゲーム内要素の部品
│   ├── Collider.h      # 衝突判定データ
│   └── AnimationController.cpp
├── Entitie/            # ゲームオブジェクト
│   ├── Player.cpp      # 物理演算・状態管理
│   └── Enemy.cpp       # データ駆動による生成
├── System/             # 各種マネージャ
│   ├── AssetSystem.cpp # リソース管理
│   ├── CollisionSystem.cpp
│   └── EnemyDataSystem.cpp # JSON解析
├── World/              # ステージ管理
│   └── Stage.cpp       # Tiledマップの読み込み
└── GameScene.cpp       # メインゲームループ
```
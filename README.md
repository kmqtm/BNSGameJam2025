# 作品名: シンカイサンタ

![C++](https://img.shields.io/badge/Language-C%2B%2B-00599C?style=flat-square)
![Framework](https://img.shields.io/badge/GameEngine-Siv3D-E34F26?style=flat-square)
![Status](https://img.shields.io/badge/Status-Completed-success?style=flat-square)

* シンプルで簡単操作の縦スクロール2Dアクションゲームです．
* 拡張性と保守性を意識して，C++とSiv3Dで制作しました．



## 作品概要
* ジャンル: 縦スクロール2Dアクション
* 開発環境: Visual Studio 2022
* 使用言語: C++20
* ゲームエンジン: Siv3D v0.6.16
* 制作期間: 約1ヶ月 (2025/10/18~)

## 技術的なこだわり
本作品の制作時に注力した設計ポイントを紹介します．

### 1. StrategyパターンによるAI設計
敵キャラクターの行動ロジックを `IBehaviorStrategy` インターフェースとして分離して実装しました．
`Enemy` クラス本体を変更することなく，新しい行動パターンを容易に追加できる設計となっています．

* Code: [`src/AI/IBehaviorStrategy.h`](src/AI/IBehaviorStrategy.h), [`src/AI/BehaviorStrategies.cpp`](src/AI/BehaviorStrategies.cpp)

### 2. JSONによるデータ駆動
敵キャラクターのパラメータ(速度，物理サイズ，アニメーション設定など)をJSONファイルに分離しました．
コンパイル不要でゲームバランスの調整が可能となっています．

* Code: [`src/System/EnemyDataSystem.cpp`](src/System/EnemyDataSystem.cpp)
* Data: [`App/asset/EnemyData.json`](App/asset/EnemyData.json)

### 3. 責務の分離とシステムアーキテクチャ
「単一責任の原則」に基づいて，機能ごとに管理クラスが独立するように設計しました．
`GameScene` が巨大化するのを防ぎ，各システムの結合度を下げています．

* CollisionSystem: エンティティ間の衝突判定管理(O(N)の回避とフィルタリング)
* CameraSystem: プレイヤー追従と座標変換
* UISystem: 縦スクロールの進行度やステータスに応じた描画管理

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
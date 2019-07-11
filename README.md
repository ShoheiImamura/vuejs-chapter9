# vuejs-chapter9


## 9章概要

+ コンポーネント設計
+ 状態モデリングとデータフローの設計
+ ルーティング設計

## コンポーネント設計

コンポーネント設計とは
- どの粒度でコンポーネントとしてWebアプリケーションの要素を切り出すか、
- それぞれのコンポーネントを他のコンポーネントと同関連させるか

### コンポーネント設計のメリット
- 一貫性
- 重複排除
- 生産性向上

### 一貫性
- アプリケーション全体で統一感のある、コンポーネントを開発できる

### 重複排除
- 同じような機能を提供するコンポーネント開発を排除できる

### 生産性向上
- チームで開発するコンポーネントの大枠を共有することで、開発効率を高められる

### コンポーネント設計の進め方

- まずは、作成するアプリケーションのワイヤーフレームレベルのUIイメージから Atomic Design に準拠した設計手法によるコンポーネントの抽出
- ここのコンポーネントの設計
- 他のコンポーネントとの協調
- 再利用を想定したコンポーネントのAPI設計

### Atomic Design とは

Atoms、Molecules, Organisms、Templates, Pages という5つの段階に分けて、コンポーネントを管理するデザイン手法

| コンポーネント | 説明                                                                                                   |
|----------------|--------------------------------------------------------------------------------------------------------|
| Atoms          | UIを構成するための基本的な要素、機能的にこれ以上分割できない                                           |
| Molecules      | Atomsを組み合わせて作られた要素                                                                        |
| Organisms      | Atoms, Molecules, Organisms を組み合わせて作られた要素                                                 |
| Templates      | Molecules や Organisms などを組み合わせて作られた Pages のテンプレートとなるのも、要はワイヤーフレーム |


## Atomic Design によるコンポーネントの抽出 

![atomic design](http://atomicdesign.bradfrost.com/images/content/atomic-design-process.png)
http://atomicdesign.bradfrost.com/chapter-2/






抽出したコンポーネント
| 種類      | コンポーネント                                                                                                 |
|-----------|----------------------------------------------------------------------------------------------------------------|
| Atoms     | ラベル、テキストボックス、ボタン、アイコン                                                                     |
| Molecules | ログインフォーム、ボードナビゲーション、タスクリストヘッダー、タスクフォーム、タスクカード、タスク詳細フォーム |
| Organisms | タスクリスト、ボードタスク                                                                                     |
| Templates | ログインビュー、ボードビュー、タスク詳細モーダル                                                               |
| Pages     | ログインページ、ボードページ、タスク詳細ページ                                                                 |


### 単一ファイルコンポーネント化

| 抽出コンポーネント   | 単位      | SFC名              |
|----------------------|-----------|--------------------|
| ラベル               | Atoms     | -                  |
| テキストボックス     | Atoms     | -                  |
| ボタン               | Atoms     | KbnButton          |
| アイコン             | Atoms     | KbnIcon            |
| ログインフォーム     | Molecules | KbnLoginForm       |
| ボードナビゲーション | Molecules | KbnBoardNavigation |
| タスクリストヘッダー | Molecules | KbnTaskListHeader  |
| タスクフォーム       | Molecules | KbnTaskForm        |
| タスクカード         | Molecules | KbnTaskCard        |
| タスク詳細フォーム   | Molecules | KbnTaskDetailForm  |
| ボードタスク         | Organisms | KbnBoardTask       |
| タスクリスト         | Organisms | KbnTaskList        |
| ログインビュー       | Templates | KbnLoginView       |
| ボードビュー         | Templates | KbnBoardView       |
| タスク詳細モーダル   | Templates | KbnTaskDetailModal |

### ディレクトリ構成

```
└─components
    ├─atoms
    │      KbnButton.vue
    │      KbnIcon.vue
    │
    ├─molecules
    │      KbnBoardNavigation.vue
    │      KbnLoginForm.vue
    │      KbnTaskDetailModal.vue
    │
    ├─organisms
    │      KbnBoardTask.vue
    │      KbnTaskList.vue
    │
    └─templates
            KbnBoardView.vue
            KbnLoginView.vue
            KbnTaskDetailModal.vue
```

### 単一ファイルコンポーネント化しないもの

- Pages  
Templates にコンテンツが挿入されて実態化しただけなため

- ラベル、テキストボックス  
HTML 要素として、対応可能であるため

### コンポーネントのAPI 

- プロパティ、イベント、スロット（slot要素）の順に、APIを検討する

### KbnButton 今ポーンとのAPI

#### プロパティ

| プロパティ | 説明                 | 型     | 受け付け可能な値 | デフォルト |
|------------|----------------------|--------|------------------|------------|
| type       | ボタンの種別         | 文字列 | text / button    | button     |
| disabled   | ボタンが無効化どうか | 真偽値 | false / true     | false      |

#### イベント

| イベント | 説明                     |
|----------|--------------------------|
| click    | ボタンのクリックイベント |

#### スロット

| スロット | 説明               |
|----------|--------------------|
| -        | ボタンのコンテンツ |
※ 「-」は名前無しのスロット(単一スロット)

## StoryBook

### Storybook でできること
- UI 開発環境を提供するツール
  - コンポーネントの input 要素や button 要素などのユーザーとやり取りするインタラクションの確認
  - コンポーネントの CSS, フォント、画像などの UI の見た目の確認
  - コンポーネントを一覧にしてカタログ化することで、プロジェクトで利用するコンポーネントの一望
  - コンポーネントの API 仕様やスタイルガイドの確認
  - コンポーネントで公開されているプロパティを Storybook 上で動的に変更して挙動のチェック
  - コンポーネントのスナップショットによって UI 構造についてテスト
  - 平易な GUI 操作画面によるエンジニアとデザイナーとの協業、分業や意思疎通の効率アップ

https://storybook.js.org/

### 開発プロジェクトへの導入

```shell
npm install -g @storybook/cli
getstorybook -V
5.1.9
```

```
# 開発プロジェクトへ移動
cd /path/to/kanban-app
getstorybook init
```

ディレクトリ構造
```shell
.
├─.storybook
│      config.js
│      addon.js
```

package.json にも追加
```json

  "scripts": {
    "storybook": "start-storybook -p 6006"
    ...
  },

  "devDependencies": {
    "@babel/core": "^7.5.4",
    "@storybook/addon-actions": "^5.1.9",
    "@storybook/addon-links": "^5.1.9",
    "@storybook/addons": "^5.1.9",
    "@storybook/vue": "^5.1.9"
    ...
  },
```

### storybook を動作させる

```shell
npm run storybook
> kanban-app@0.1.0 storybook C:\Users\owner\Documents\stydy\vue\weeyble\vuejs-chapter9\kanban-app
> start-storybook -p 6006

info @storybook/vue v5.1.9
info
info => Loading presets
info => Loading presets
info => Loading custom babel config as JS
info => Loading custom babel config as JS
info => Loading custom addons config.
info => Using default webpack setup.
webpack built 5ecca54d73ff31489c9c in 11519ms
╭───────────────────────────────────────────────╮
│                                               │
│   Storybook 5.1.9 started                     │
│   14 s for manager and 14 s for preview       │
│                                               │
│   Local:            http://localhost:6006/    │
│   On your network:  http://10.0.75.1:6006/    │
│                                               │
╰───────────────────────────────────────────────╯
```

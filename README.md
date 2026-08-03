# kids-games

子ども専用のゲーム置き場。Cloudflare Pages にフォルダごとアップロードして使います。

## フォルダ構成

```
kids-games/
├── index.html            ← ダッシュボード（ゲーム選択画面）
├── games.json            ← ゲーム一覧の設定。ここだけ編集する
├── apple-touch-icon.png  ← ホーム画面用アイコン（おうち）
├── README.md
└── ohana-atsume/         ← ゲーム1つにつきフォルダ1つ
    ├── index.html
    └── apple-touch-icon.png
```

デプロイすると `https://〇〇.pages.dev/` がダッシュボード、`https://〇〇.pages.dev/ohana-atsume/` が個別のゲームになります。

## 新しいゲームを追加する手順

1. `kids-games/` の直下に新しいフォルダを作る（例：`neko-memory`）
2. その中にゲーム本体を **`index.html`** という名前で置く
3. `games.json` の `games` 配列に1ブロック追記する

```json
{
  "title": "ねこめくり",
  "subtitle": "おなじ ねこを さがそう",
  "folder": "neko-memory",
  "icon": "cat",
  "color": "mint",
  "hidden": false
}
```

4. `kids-games` フォルダをまるごと再アップロードする

### 各項目の意味

| 項目 | 内容 |
|---|---|
| `title` | カードに大きく出る名前。ひらがな推奨 |
| `subtitle` | 小さい説明文。省略可 |
| `folder` | フォルダ名。ここにリンクする |
| `icon` | 下記12種類から選ぶ |
| `color` | 下記6色から選ぶ |
| `hidden` | `true` にすると一覧から隠れる（作りかけのとき用） |

**icon**：`flower` `heart` `star` `candy` `paint` `music` `puzzle` `ball` `cake` `rocket` `cat` `rainbow`

**color**：`pink` `mint` `lemon` `sky` `lilac` `peach`

上に書いたものほど画面の左上に並びます。ゲームが3つ以下のときは「じゅんびちゅう」のカードで埋まります。

## 新しいゲームを作るときの決まりごと

既存のゲームに合わせておくと統一感が出ます。

- ファイル名は必ず `index.html`
- `<head>` に `<meta name="robots" content="noindex, nofollow, noarchive">` を入れる
- 一時停止画面に `<a href="../">🏠 おうち</a>` を置いてダッシュボードに戻れるようにする
- タッチ操作は `touch-action:none` と `pointerdown` / `pointermove` で処理する
- 文字はすべてひらがな。ゲームオーバーやペナルティは作らない

## デプロイ

1. [dash.cloudflare.com](https://dash.cloudflare.com) → **Workers & Pages** → **Create** → **Pages** → **Upload assets**
2. プロジェクト名は推測されにくいものにする（URLになるため）
3. `kids-games` フォルダをまるごとドラッグ＆ドロップ
4. 更新するときは同じプロジェクトの **Create new deployment** から再アップロード

iPad の Safari で開き、共有ボタン →「ホーム画面に追加」でアプリのように起動できます。

## メモ

- 全ページに `noindex` を入れてあるため検索エンジンには載りません
- 外部への通信は一切ありません。データ収集もしていません
- ローカルで `index.html` を直接開くと `games.json` が読めず仮の一覧になります。動作確認はデプロイ後の URL か、`python3 -m http.server` で行ってください

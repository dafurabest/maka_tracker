# 🀄 MAKA評価管理アプリ

雀魂（じゃんたま / Mahjong Soul）のMAKA評価と対局順位を記録・統計分析するブラウザアプリです。

**デモ:** [https://maka-tracker.pages.dev/]

---

## 概要

雀魂のAI評価システム「MAKA」は、牌譜を分析して S+ 〜 E の評価を出してくれます。しかし、麻雀はゲームの性質上、高評価の手順を踏んでも必ずしも高順位になるとは限りません。

このアプリでは **対局ごとにMAKA評価と順位を記録・蓄積し**、両者の相関を統計的に分析することで、自分の打ち筋と実力に対するヒントを得ることができます。

---

## 機能

### 入力
- 必須項目：日付・時刻 / 戦型（東風戦・半荘戦）/ 対局ルーム / 順位（1〜4）/ MAKA評価（S+〜E）
- 任意項目：最終持ち点 / 和了回数 / 放銃回数 / 牌譜URL / メモ

### 統計分析
- 戦型 × ルームランクの2軸フィルター
- MAKA評価分布グラフ（100%積み上げ横棒）
- 順位別 MAKA評価分布グラフ
- MAKA評価ごとの順位統計（平均・中央値・最頻値）
- 順位ごとの MAKA評価統計（平均・中央値・最頻値）
- ルームランク別の集計（平均MAKA・平均順位）
- 任意入力がある場合：MAKA評価ごとの平均持ち点・和了回数・放銃回数

### データ管理
- ブラウザの localStorage に自動保存（オフライン動作）
- JSON エクスポート / インポート（追記 or 上書きを選択）
- 対局記録の個別削除・全件削除

---

## 対応ゲームモード

| モード | 対応 |
|--------|------|
| 4人麻雀 段位戦（東風戦・半荘戦） | ✅ |
| 4人麻雀 一局戦 | ❌ |
| 3人麻雀 | ❌（初期リリース非対応） |
| イベント・特殊ルール戦 | ❌ |

---

## 技術スタック

| 項目 | 内容 |
|------|------|
| フロントエンド | HTML / CSS / Vanilla JavaScript |
| グラフ描画 | [Chart.js 4.4.1](https://www.chartjs.org/)（CDN） |
| フォント | [Kaisei Decol](https://fonts.google.com/specimen/Kaisei+Decol) / [Noto Sans JP](https://fonts.google.com/noto/specimen/Noto+Sans+JP)（Google Fonts） |
| データ保存 | localStorage（サーバー不要・完全クライアントサイド） |
| ホスティング | Cloudflare Pages |

外部依存はChart.jsとGoogle Fontsのみ。バックエンド・データベース不要のシングルファイルアプリです。

---

## セットアップ

### ローカルで確認する

```bash
git clone https://github.com/dafurabest/maka-tracker.git
cd maka-tracker
# ブラウザで index.html を直接開くだけで動作します
open index.html
```

> ⚠️ `file://` プロトコルでは localStorage が利用できるブラウザと利用できないブラウザがあります。正確に動作を確認したい場合は、ローカルサーバーを使ってください。
>
> ```bash
> # Python 3 の場合
> python -m http.server 8080
> # → http://localhost:8080 でアクセス
> ```

### Cloudflare Pages にデプロイする

1. このリポジトリを GitHub に push する
2. [Cloudflare Pages](https://pages.cloudflare.com/) にログイン
3. **「Create a project」→「Connect to Git」** でリポジトリを選択
4. ビルド設定：
   - **Framework preset**: `None`
   - **Build command**: （空白のまま）
   - **Build output directory**: `/`（または空白）
5. **「Save and Deploy」** → 数秒でデプロイ完了

---

## リポジトリ構成

```
maka-tracker/
├── index.html      # アプリ本体（すべて1ファイル完結）
└── README.md       # このファイル
```

---

## データ形式

エクスポートされるJSONの形式は以下の通りです。インポートによる引き継ぎやバックアップに利用できます。

```json
[
  {
    "id": "1700000000000_abc123",
    "date": "2024-11-15",
    "time": "21:30",
    "type": "半荘戦",
    "room": "玉の間",
    "rank": 2,
    "maka": "A+",
    "pts": 28400,
    "win": 2,
    "deal": 1,
    "url": "https://mahjongsoul.game.yo-star.com/...",
    "memo": "東場の立直判断がよかった"
  }
]
```

| フィールド | 型 | 必須 | 説明 |
|---|---|---|---|
| `id` | string | ✅ | 一意ID（重複チェックに使用） |
| `date` | string | ✅ | 対局日（YYYY-MM-DD） |
| `time` | string | ✅ | 対局時刻（HH:MM） |
| `type` | string | ✅ | `"東風戦"` or `"半荘戦"` |
| `room` | string | ✅ | `"銅の間"` / `"銀の間"` / `"金の間"` / `"玉の間"` / `"王座の間"` |
| `rank` | number | ✅ | 1〜4 |
| `maka` | string | ✅ | `"S+"` 〜 `"E"` |
| `pts` | number \| null | | 最終持ち点（素点） |
| `win` | number \| null | | 和了回数 |
| `deal` | number \| null | | 放銃回数 |
| `url` | string \| null | | 牌譜URL |
| `memo` | string \| null | | メモ |

---

## ライセンス

MIT License

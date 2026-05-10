# 引き継ぎ書 — K-lemon

最終更新: 2026-05-10 / 現行バージョン: v1.7

---

## 概要

千葉県香取市の気候(冬の最低気温 -3〜-5℃)を前提とした、初心者向けレモン家庭菜園ガイドの 1 ページサイト。鉢植え推奨、5月スタート版。本文に加えて植え付け手順のスライド版を併設。

- 公開URL: https://windom21-cpu.github.io/k-lemon/
- スライド: https://windom21-cpu.github.io/k-lemon/planting.html
- 成長記録: https://windom21-cpu.github.io/k-lemon/journal.html
- QRページ: https://windom21-cpu.github.io/k-lemon/qr.html
- リポジトリ: https://github.com/windom21-cpu/k-lemon (public)
- ローカル: `/home/sk/デスクトップ/K-lemon/`(リポ名 `k-lemon` と大文字小文字が違う点に注意)

---

## ファイル構成

```
K-lemon/
├─ index.html          ← 本体ガイド(全 9 章 + QR セクション)
├─ planting.html       ← 植え付け手順スライド(全 12 枚)
├─ journal.html        ← 成長記録(マイヤー・璃の香、新しい順)
├─ qr.html             ← QR ラッパー(favicon を保つため)
├─ qr.png              ← サイト URL の QR コード
├─ favicon.svg         ← マスター(レモン色 #F4E04D 背景に K + Slice)
├─ favicon-16x16.png
├─ favicon-32x32.png
├─ favicon-48x48.png
├─ favicon-512x512.png ← PWA / マニフェスト用(現状 manifest.json は未配置)
├─ apple-touch-icon.png
├─ photo/              ← 写真の原本(YYMMDD品種.JPG、コミット対象)
│  └─ web/             ← Web 用に縮小したコピー(長辺 1400px / JPEG q85)。journal.html はこちらを参照
├─ .gitignore          ← `.claude/` を除外
└─ 引き継ぎ書-k-lemon.md
```

ビルド工程なし。`index.html` などを編集して push するだけで GitHub Pages が反映(通常 30〜60 秒、たまに 7 分以上 queued になる)。

---

## 技術スタック

- 純 HTML4 風レトロ + 最小限の CSS / JavaScript
- フォント: `MS UI Gothic, Osaka, sans-serif` 12px(本文)、見出しのみ 13〜16px
- リンク色: `#00f` / 訪問済 `#808`(`a[href]` のみに適用)
- レイアウト: `<table border=1 cellpadding=3>` + `<hr>` + `<center>` の HTML4 風
- アンカー: 全て `<a id="X">`(`<a name>` ではない)、ジャンプ時は CSS `:target` で `#ff9` ハイライト
- チェックボックス: 98.css(jdan/98.css)からチェックボックス関連ルールのみインライン抽出
  - ネイティブ input は `appearance:none + opacity:0 + position:absolute + pointer-events:none` で非表示
  - 隣接 `<label for="cN">` の `:before` で 13×13 へこみ枠、`:checked + label:after` でインライン SVG の黒チェック
- 永続化: `localStorage` キー `k-lemon-checks-v1` に `{c1: true, c2: false, ...}` で保存
- スライド: CSS のみのスライドショー(`.slide:target{display:block}` + `body:not(:has(.slide:target)) #s1{display:block}`)、キーボード操作だけ最小 JS
- favicon: SVG マスター + PNG 各サイズ + apple-touch
- フッター: `position:fixed; bottom:0` で常時表示、バージョン表記でブラウザキャッシュ確認用

---

## ページ構成

### index.html(全 10 セクション)

| # | セクション | 内容 |
|---|---|---|
| 0 | 最初に読むこと | 性質 / 鉢 vs 地植え / 置き場所 / 1年目の心構え |
| 1 | 準備するもの | 必須 11 品目 + あると便利 8 品目(チェックリスト形式)|
| 2 | 品種の比較 | マイヤー / リスボン / ユーレカ / 璃の香 / ジャンボ |
| 3 | 植え付けの手順 | 10 ステップ表(冒頭にスライド版へのリンクあり)|
| 4 | 毎日・毎週やること | 日次チェック / 水やり基本 / 季節別頻度 |
| 5 | 月別カレンダー | 5月〜翌4月、各月にチェック行 |
| 6 | 病害虫早見表 | アゲハ / ハダニ / カイガラ / すす / そうか 等 |
| 7 | トラブル対処 | 葉 / 木全体 / 花実 / 土鉢 別 |
| 8 | 初心者がやりがちな失敗 | 水・肥料・置き場所・剪定・苗・病害虫の 6 区分 |
| 9 | 用語集 | 接ぎ木苗 / 台木 / ウォータースペース 等 22 用語 |
| 10 | このページの QR コード | qr.png を表示、qr.html にリンク |

### journal.html(成長記録)

**品種別** に時系列で経過を記録するページ。品種ごとに 1 セクション・1 テーブルで、行が新しい順に積み上がる構造。表は `margin:0 auto` で画面中央配置、`<table border=1 cellpadding=3>` の二重線レトロ枠を維持(`border-collapse` は使わない)。

**1 エントリ = 2 行**。日付セルを `rowspan="2"` で縦に伸ばし、右側だけ上下に分けて 1 行目に写真(`<img class="photo">`、表示幅 480px)、2 行目に素のメモ文。これにより写真とメモの境目も外枠と同じ罫線で揃い、テーブルのネスト不要。

`:target` ハイライトは `tr:target td, tr:target + tr td{background:#ff9}` で id 付き行とその次のメモ行の両方を黄色に。

写真は `photo/web/` の縮小版(長辺 1400px / JPEG q85)を表示、クリックで別タブ拡大。原本は `photo/` に保存。

| 品種セクション | アンカー | 行アンカー命名規則 |
|---|---|---|
| マイヤー(Meyer)  | `#meyer`  | `#meyer-YYMMDD`  |
| 璃の香(Rinoka)  | `#rinoka` | `#rinoka-YYMMDD` |

`tr:target td{background:#ff9}` を CSS に入れているので、行アンカーへ飛ぶとその 1 行が黄色マーカーで強調される。

新エントリ追加手順:
1. 写真原本を `photo/YYMMDD<品種>.JPG` で配置
2. `python3` + Pillow で `photo/web/<同名>.jpg` を生成(長辺 1400px / quality 85)
3. 該当品種テーブルのヘッダ行 `<tr><th>日付<th>写真とメモ` の **直下** に 2 行 1 組のエントリを挿入(常に最新が上):

   ```html
   <tr id="<品種>-YYMMDD">
   <td rowspan="2">YYYY-MM-DD<br>(出来事)</td>
   <td><a href="photo/web/<ファイル>.jpg"><img src="photo/web/<ファイル>.jpg" alt="..." class="photo"></a></td>
   <tr><td>メモ本文</td>
   ```
4. 新品種を増やす場合は、目次・新セクション・フッターのショートカットの 3 箇所にアンカーを追加

リサイズ用ワンライナー(Pillow):

```bash
python3 -c "
from PIL import Image, ImageOps; import os, sys
src='photo/'+sys.argv[1]; dst='photo/web/'+os.path.splitext(sys.argv[1])[0]+'.jpg'
im=ImageOps.exif_transpose(Image.open(src)); w,h=im.size
s=1400/max(w,h)
if s<1: im=im.resize((int(w*s),int(h*s)), Image.LANCZOS)
if im.mode!='RGB': im=im.convert('RGB')
im.save(dst,'JPEG',quality=85,optimize=True,progressive=True)
print(dst, im.size)" '<ファイル名>.JPG'
```

### planting.html(全 12 スライド)

| 枚 | タイトル | 図 |
|---|---|---|
| 1 | はじめに(日選び・心構え) | 太陽 + 温度計 |
| 2 | 鉢底の準備 | 鉢断面 + ネット + 鉢底石 |
| 3 | 培養土を入れる | 鉢断面に 1/3 の土 |
| 4 | 苗をポットから出す | ポット → 矢印 → 逆さポット |
| 5 | 根を確認 | 根鉢の側面 + 巻き根 |
| 6 | 苗を鉢に置く | 接ぎ木部分マーカー(赤) |
| 7 | 土を入れる | 割り箸で隙間を埋める |
| 8 | ウォータースペース確保 | 鉢縁から 2〜3cm の余白 |
| 9 | 支柱を立てる | 8の字結び |
| 10 | たっぷり水やり | ジョウロ + 水滴 + 鉢底排水 |
| 11 | 半日陰で養生 | 軒下 + 鉢 + 直射 NG |
| 12 | 植え付け後 2 週間の NG 行動 | 禁止マーク + 4 項目 |

各スライドはインライン SVG(viewBox 中心、`stroke=#000 stroke-width=2 fill=none` 基本)。修正時は該当 `<svg>` ブロックを直接編集。

---

## 使い方(訪問者視点)

- **チェックリスト**: 各表の ✓ 列をクリック → 状態は同一ブラウザの localStorage に保存。フッター「□ リセット」で全クリア(確認ダイアログあり)
- **アンカージャンプ**: 目次や本文中の青リンクをクリック → 該当箇所が黄色マーカーで点灯。ブラウザの戻る/進むで自動的にマーカーが切り替わる
- **QR コード**: フッター「■ QR」 または セクション 10 から。サムネクリックで `qr.html` を開く(直接 png を開くと別サイトの favicon が見えてしまうのを避けるためラッパー経由)
- **植え付けスライド**: セクション 3 冒頭リンクから。← → / PageUp PageDown / Space / Home / End で操作。ブラウザの印刷で 1 スライド = 1 ページの PDF にできる(`@media print` 設定済み)

---

## 開発・運用

### ローカルで確認

```bash
cd ~/デスクトップ/K-lemon
python3 -m http.server 8000
# → http://localhost:8000/ で開く
```

### バージョン管理

セマンティックバージョニング(`vX.Y`):
- `Y` 上げ: コンテンツ追加・UI改修
- `X` 上げ: 大規模リニューアル(現状未経験)

**更新箇所**(全ファイル同期させること):
- `index.html` フッターの `v1.X (YYYY-MM-DD)`
- `qr.html` 末尾の `v1.X (YYYY-MM-DD)`
- `planting.html` フッターの `v1.X (YYYY-MM-DD)`
- 本書末尾の履歴

ブラウザキャッシュ確認用にフッター常時表示しているので、デプロイ後は Shift+リロードで「v1.X」が更新されているか目視確認する運用。

### デプロイ

```bash
git add -A
git commit -m "vX.Y: 概要"
git push
# Pages ビルド完了確認:
gh api repos/windom21-cpu/k-lemon/pages/builds/latest --jq '.commit[0:7] + " " + .status'
```

GitHub Actions の `pages-build-deployment` ワークフローが走る(通常 40〜60 秒)。queued が長引く場合は GitHub 側のキュー混雑なので待つ。

### コンテンツ追加のコツ

- **新セクション追加**: 目次 `<ul>` に 1 行 + `<h2><a id="xxx">■ N. タイトル</a></h2>` で作成 → `:target` ハイライトが自動で効く
- **チェックボックス追加**: マークアップは `<input type="checkbox" id="cN" data-ck="cN"><label for="cN"></label>`(N は通し番号、現在 c55 まで使用済み)。ID 重複禁止。新たに追加するときは c56 から
- **SVG 図解**: 既存スライドの SVG をコピーして数値を変えるのが早い。`viewBox` は 200×140 〜 280×200 程度が標準

### 注意点 / Gotcha

- `<a name="X">` は v1.5 で全部 `<a id="X">` に変換済み。古いスタイルで追加しないこと
- リンク色は `a[href]` のみに限定。`<a id>` で囲んでも青くならない(`a:not([href]){color:inherit}`)
- localStorage キー `k-lemon-checks-v1` を変える場合は、バージョン番号を `v2` 等に上げること(でないと既存ユーザーのチェックが失われる)
- favicon を変えたら、ブラウザは origin (`windom21-cpu.github.io`) 単位でキャッシュするので k-books 等の他サイトの favicon と混ざることがある。`qr.png` を直接開くと特に発生する → `qr.html` ラッパー経由で回避済み

---

## バージョン履歴

- **v1.0** (2026-05-06) — 初版。`lemon_guide (2).html` を `index.html` にリネーム、favicon をルートに配置、GitHub Pages 公開
- **v1.0.1** (2026-05-06) — QR コード `qr.png` 追加
- **v1.1** (2026-05-06) — フッター固定表示 + バージョン表記、チェックリスト列幅明示、QR ラッパー `qr.html` 新設
- **v1.2** (2026-05-06) — `□` 文字を 55 個の `<input type=checkbox>` に置換、localStorage で永続化、リセットリンク追加
- **v1.3** (2026-05-06) — リセット動作修正(bfcache 対策)、タイトルを `K-lemon` に変更しサブタイトルとして従来タイトルを表示
- **v1.4** (2026-05-06) — チェックボックスを 98.css 風 Win98 UI に(98.css のチェックボックス関連ルールのみインライン抽出)
- **v1.5** (2026-05-06) — `<a name>` を全て `<a id>` に変換(51 箇所)、リンク先の文字色を黒に、`:target` で黄色マーカー
- **v1.6** (2026-05-06) — `planting.html` 新設(全 12 スライド、SVG 図解付き、キーボード操作 + 印刷対応)
- **v1.7** (2026-05-10) — `journal.html` 新設(成長記録ページ)。マイヤー・璃の香の植え付け写真を初回エントリとして追加。`photo/` に原本、`photo/web/` に Web 用縮小版(長辺 1400px / JPEG q85)。`index.html` 目次とフッターから記録ページへリンク追加。全 HTML に `<meta charset="utf-8">` を追加(GitHub Pages では問題なかったが、ローカル `python3 -m http.server` で文字化けしていたため)

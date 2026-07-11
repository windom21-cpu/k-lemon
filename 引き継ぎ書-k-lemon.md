# 引き継ぎ書 — K-lemon

最終更新: 2026-07-11 / 現行バージョン: v2.2

---

## 概要

千葉県香取市の気候(冬の最低気温 -3〜-5℃)を前提とした、初心者向けレモン家庭菜園ガイドの 1 ページサイト。鉢植え推奨、5月スタート版。本文に加えて植え付け手順のスライド版と、自宅で実際に育てている木の成長記録ページを併設。

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

### index.html(全 11 セクション)

| # | セクション | 内容 |
|---|---|---|
| 0 | 最初に読むこと | 性質 / 鉢 vs 地植え / 置き場所 / 1年目の心構え |
| 1 | 準備するもの | 必須 11 品目 + あると便利 8 品目(チェックリスト形式)|
| 2 | 品種の比較 | マイヤー / リスボン / ユーレカ / 璃の香 / ジャンボ |
| 3 | 植え付けの手順 | 10 ステップ表(冒頭にスライド版へのリンクあり)|
| 4 | 毎日・毎週やること | 日次チェック / 水やり基本 / 季節別頻度 |
| 5 | 追肥(肥料)の早見表 | 鉢植えの考え方 / 時期ごとの追肥表 / 量の具体例 / 注意点(参照テーブル、チェックボックスなし)|
| 6 | 月別カレンダー | 5月〜翌4月、各月にチェック行 |
| 7 | 病害虫早見表 | アゲハ / ハダニ / カイガラ / すす / そうか 等 |
| 8 | トラブル対処 | 葉 / 木全体 / 花実 / 土鉢 別 |
| 9 | 初心者がやりがちな失敗 | 水・肥料・置き場所・剪定・苗・病害虫の 6 区分 |
| 10 | 用語集 | 接ぎ木苗 / 台木 / ウォータースペース 等 22 用語 |
| 11 | このページの QR コード | qr.png を表示、qr.html にリンク |

### journal.html(成長記録 / 品種比較)

**日付を横帯にして、同じ日のマイヤーと璃の香を左右 2 カラムで並べる** 比較レイアウト(v2.1 で「品種別に 1 テーブル」から変更)。1 つの `<table border=1 cellpadding=3>` に、① 品種見出し行(`<th id="meyer">` / `<th id="rinoka">`、各 `width="50%"`)→ 以降は日付ごとに ② 日付バンド行(`<tr id="d-YYMMDD"><td colspan="2" class="datehdr">`)→ ③ 内容行(左右 `<td class="cul">` に写真+メモ)を繰り返す。新しい日付が上。

- 内容セルは「写真(`<img class="photo">`)→ `<br>` → メモ文」を 1 つの `td` に縦積み。写真は `width:100%;max-width:520px` でカラム(画面の約半分)いっぱいに表示。各カラムが約 50% 幅なので狭い画面でも横並びを維持する。
- 片方の品種だけ記録がある日は、もう片方を `<td class="cul nodata">—<br><small>(この日は○○の記録なし)</small></td>` で埋める。
- `:target` ハイライトは `tr:target td, tr:target + tr td{background:#ff9}`。日付バンド行の `id="d-YYMMDD"` へジャンプすると、バンド行+直後の内容行が黄色に光る。
- 写真は `photo/web/` の縮小版(長辺 1400px / JPEG q85)を表示、クリックで別タブ拡大。原本は `photo/` に保存。

| アンカー | 指すもの |
|---|---|
| `#meyer` / `#rinoka` | 品種の列見出し(th)。フッター・目次のショートカット先 |
| `#d-YYMMDD` | その日付の行(目次から。例 `#d-260704`)|

新エントリ追加手順:
1. 写真原本を `photo/YYMMDD<品種>[出来事].JPG` で配置(例 `260704マイヤー摘花.JPG`)
2. `python3` + Pillow で `photo/web/<同名>.jpg` を生成(長辺 1400px / quality 85)
3. **その日付の行が既にあるか**で分岐:
   - 無ければ、品種見出し行の **直下** に日付バンド行+内容行を新規挿入(常に最新が上):

     ```html
     <tr id="d-YYMMDD"><td colspan="2" class="datehdr"><b>YYYY-MM-DD</b>(出来事)</td></tr>
     <tr>
     <td class="cul"><a href="photo/web/<マイヤー>.jpg"><img src="photo/web/<マイヤー>.jpg" alt="..." class="photo"></a><br>メモ</td>
     <td class="cul"><a href="photo/web/<璃の香>.jpg"><img src="photo/web/<璃の香>.jpg" alt="..." class="photo"></a><br>メモ</td>
     </tr>
     ```

     記録が片方だけなら、もう片方の `td` を `class="cul nodata"` の「記録なし」にする。
   - 既にあれば、その日付行の該当品種セル(`nodata` プレースホルダ)を写真+メモに差し替える。
4. 目次(日付リスト)に `<li><a href="#d-YYMMDD">YYYY-MM-DD(出来事)</a> … 品種</li>` を追加
5. 3 品種目を増やすときは列追加=中規模改修(見出し th 追加・全内容行に td 追加・`td.cul{width}` 調整)になる点に注意

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
- **成長記録**: 目次の「▲ 成長記録」または フッター「■ 記録」から `journal.html` へ。品種別(マイヤー / 璃の香)に時系列で写真とメモを表示。写真クリックで縮小版を別タブ拡大表示

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
- `journal.html` フッターの `v1.X (YYYY-MM-DD)`
- 本書冒頭の「最終更新 / 現行バージョン」と末尾の履歴

ブラウザキャッシュ確認用にフッター常時表示しているので、デプロイ後は Shift+リロードで「v1.X」が更新されているか目視確認する運用。

### デプロイ

```bash
# PDF 等の作業用ファイルが ?? に残っていることがあるので、対象ファイルを明示するのが安全
git add index.html planting.html journal.html qr.html photo/ 引き継ぎ書-k-lemon.md
git commit -m "vX.Y: 概要"
git push
# Pages ビルド完了確認:
gh api repos/windom21-cpu/k-lemon/pages/builds/latest --jq '.commit[0:7] + " " + .status'
```

GitHub Actions の `pages-build-deployment` ワークフローが走る(通常 40〜60 秒)。queued が長引く場合は GitHub 側のキュー混雑なので待つ。`git add -A` だとリポジトリ直下に残っている `*.pdf`(印刷用エクスポート等)を巻き込むので使わないこと。

### コンテンツ追加のコツ

- **新セクション追加**: 目次 `<ul>` に 1 行 + `<h2><a id="xxx">■ N. タイトル</a></h2>` で作成 → `:target` ハイライトが自動で効く
- **チェックボックス追加**: マークアップは `<input type="checkbox" id="cN" data-ck="cN"><label for="cN"></label>`(N は通し番号、現在 c55 まで使用済み)。ID 重複禁止。新たに追加するときは c56 から
- **SVG 図解**: 既存スライドの SVG をコピーして数値を変えるのが早い。`viewBox` は 200×140 〜 280×200 程度が標準

### 注意点 / Gotcha

- `<a name="X">` は v1.5 で全部 `<a id="X">` に変換済み。古いスタイルで追加しないこと
- リンク色は `a[href]` のみに限定。`<a id>` で囲んでも青くならない(`a:not([href]){color:inherit}`)
- localStorage キー `k-lemon-checks-v1` を変える場合は、バージョン番号を `v2` 等に上げること(でないと既存ユーザーのチェックが失われる)
- favicon を変えたら、ブラウザは origin (`windom21-cpu.github.io`) 単位でキャッシュするので k-books 等の他サイトの favicon と混ざることがある。`qr.png` を直接開くと特に発生する → `qr.html` ラッパー経由で回避済み
- 新しい HTML を追加する場合は **必ず `<head>` 直後に `<meta charset="utf-8">`** を入れる。GitHub Pages は HTTP ヘッダで charset を補ってくれるが、ローカル `python3 -m http.server` は補わないので日本語が文字化けする(v1.7 で全 HTML に追加済み)
- 写真は **必ず `photo/web/` の縮小版を `<img src>` に指定** する。原本(`photo/*.JPG`)を直接表示するとモバイルが数 MB ダウンロードする羽目になる。原本はクリック時の拡大用にも使わず、`photo/web/` 版を別タブで開く運用

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
- **v1.8** (2026-06-13) — `journal.html` にマイヤー・璃の香の 2026-06-13(植え付けから約1か月)エントリを追加。原本 `photo/260613マイヤー.JPG` / `photo/260613りのか.JPG`、Web 版 `photo/web/` 同名 `.jpg`(長辺 1400px / q85)
- **v1.9** (2026-06-13) — 2026-06-13 エントリのメモを、写真から読み取った客観所見+懸念・注意点に拡充(マニュアル準拠ではなく実観察ベース)。マイヤー=葉色濃く新芽展開・順調、下げ札は実ではなく品種ラベル。璃の香=徒長気味で下葉に黄化、上部に新梢、要観察。メモ内は `<br><b>注意:</b>` で経過と注意を区切る書式
- **v2.0** (2026-06-21) — `index.html` に「5. 追肥(肥料)の早見表」セクションを新設(`id="feeding"`)。鉢植えの基本方針・時期ごとの追肥表(5月〜翌4月)・量の具体例・注意点の 4 ブロック。月別カレンダーに散在していた肥料情報を 1 か所に集約。病害虫早見表と同じ参照テーブル形式でチェックボックスは付けない(c55 のまま据え置き)。セクション 4 の直後に挿入し、旧 5〜10(月別カレンダー以降)を 6〜11 に繰り下げ、目次の番号も同期。アンカー id は不変なのでページ内リンクは無傷。`#fertilizer`(既存の買い物リスト項目)とは別 id
- **v2.1** (2026-07-04) — `journal.html` のマイヤーに **番外編** 2 エントリを追加。`#meyer-260701`(初開花:枝先に白花 1 輪+紫紅色のつぼみ多数)と `#meyer-260704`(摘花:ガイド「1年目の花は全部摘む」に従い開花・つぼみを全摘、回収 30〜40 輪ほど)。原本 `photo/260701マイヤー開花.JPG` / `photo/260704マイヤー摘花.JPG`、Web 版は `photo/web/` に同名 `.jpg`(長辺 1400px / q85)。日付セルは `(番外編・初開花)` `(番外編・摘花)` と明記し、通常の月次エントリと区別。摘花メモは `index.html#basic`(1年目の心構え)へリンク。あわせて `journal.html` を **品種別テーブルから日付横並び(マイヤー左・璃の香右)の比較レイアウトに全面変更**(日付バンド `#d-YYMMDD` + 2 カラム、片側のみの日は「記録なし」表示、目次を日付リスト化)。既存アンカー `#meyer`/`#rinoka` は列見出し th に付け替えて維持
- **v2.2** (2026-07-11) — `journal.html` に 2026-07-11(約2か月)エントリを追加。今回から **1 品種 2 枚**(全体+拡大)をセル内に縦積みし、各写真の下に `<center><small>▲ キャプション</small></center>` を付ける書式。原本 `photo/260711マイヤー全体.JPG` / `260711マイヤー新芽.JPG` / `260711りのか全体.JPG` / `260711りのか蕾・新芽.JPG`、Web 版 `photo/web/` 同名 `.jpg`。所見: マイヤー=摘花後も樹勢良好・葉数増だが、若葉の巻き縮れ+別の葉に白い蛇行状の筋 → **エカキムシ(ミカンハモグリガ)被害と確定**(ユーザーが実物で白い筋を確認)。メモから `index.html#hamoguri` へリンク。璃の香=細身のままだが**初の蕾**を確認(未摘、1年目方針で摘む予定)+新芽展開、幹に品種ラベル残り

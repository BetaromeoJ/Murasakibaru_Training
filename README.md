# 紫原小学校 ICT研修ポータル

薩摩中央高等学校版ICT研修ポータル（Base Design）のデザイン・レイアウト・UI・アニメーション・
情報設計を維持しながら、紫原小学校の教職員ICT研修（150分・「AIを先生の共同担任に。
明日から毎日30分早く帰れる仕事術」）に合わせて研修内容だけを再構成した、1ページ完結の
ポータルサイトです。

デザイン・レイアウト・余白・配色・角丸・アニメーション・CSS設計・JavaScript設計は
Base Designから一切変更していません。変更したのは研修内容（SESSION 0〜5・プロンプト・
活用例・FAQ・「明日からやること」）のみです。「明日からやること」の選択カードのみ、
今回のために新しく追加したコンポーネントです（既存のデザイントークンをそのまま使って
作っているため、見た目としては違和感なくなじみます）。

**デバイス前提**：紫原小学校の児童・教職員のタブレットはiPadです。Base Design
（薩摩中央高等学校版）はWindowsタブレット「Surface Go 2」の標準アプリ「サウンドレコーダー」
を録音ソースとして想定していましたが、紫原小学校版ではiPad標準搭載の「ボイスメモ」アプリに
統一しています（主にSESSION 0・SESSION 1・使い方ガイドに反映）。

## ファイル構成

```
/
├── index.html                       全SESSIONをまとめた1ページ構成
├── style.css                        共通スタイル（配色・演出はすべてここで管理）
├── script.js                        共通スクリプト（コピー・アコーディオン・
│                                     SESSION全画面ビュー・スプラッシュ・
│                                     明日からやること選択カード等）
├── README.md
└── assets/
    ├── images/
    │   ├── instructor-illustration.jpg  講師イラスト（Base Designと同一人物を想定）
    │   ├── google-trainer-badge.jpg     資格バッジ画像
    │   ├── canvassador-badge.jpg        資格バッジ画像
    │   └── book-cover.jpg               書影画像
    └── videos/
        └── instructor-animation.mp4     講師紹介の顔写真枠に埋め込むミニアニメーション（6秒・音声なし扱いで自動再生）
```

GitHub Pagesにアップロードする際は、上記ファイル・フォルダをすべてそのままリポジトリ
直下にアップロードしてください。ビルド環境は不要です。外部依存はGoogle Fonts
（M PLUS Rounded 1c／Material Symbols）のみで、CDNライブラリは使用していません。

### 画像・動画について（重要）

講師写真・資格バッジ・書影の4枚の画像と、講師紹介の動画1本は、`index.html`内に
**Base64形式で直接埋め込み**ています。`assets/images/`・`assets/videos/`フォルダにも
同じファイルを置いていますが、これらは差し替え作業用の元データであり、現在の
`index.html`はこれらのフォルダを参照していません。

埋め込みにした理由は2つあります。

1. どのような環境で開いても（プレビュー・ローカル・GitHub Pages）確実に表示されるようにするため。
2. 画像の元データを調べたところ、拡張子は`.gif`／`.png`でしたが、実体はすべてJPEG形式の画像
   でした（拡張子と中身が一致していませんでした）。これが原因で、環境によっては画像が
   表示されない場合があったため、`assets/images/`側のファイルも中身に合わせて
   `.jpg`拡張子に修正し、埋め込みデータも正しく`image/jpeg`として指定し直しています。

**講師紹介の動画について**：`#instructor`の顔写真枠は、静止画（`instructor-illustration.jpg`）
から、6秒間ループする軽いアニメーション動画（`instructor-animation.mp4`）に変更しています。
`autoplay muted loop playsinline`（自動再生・ミュート・ループ・インライン再生）で、
GIFのように音なしで繰り返し再生される設定です。動画が読み込めない・自動再生できない
環境では、`poster`属性で指定した静止画（`instructor-illustration.jpg`のBase64）が
代わりに表示されます。

画像を差し替える場合は、次のいずれかの方法で行ってください。

- **簡単な方法**：`assets/images/`内の該当ファイルを新しい画像に置き換えたうえで、
  `index.html`内の対応する`<img src="data:image/jpeg;base64,...">`を、新しい画像を
  Base64化した文字列に差し替える（`python3 -c "import base64;print('data:image/jpeg;base64,'+base64.b64encode(open('新しい画像.jpg','rb').read()).decode())"`のようなコマンドで作成できます）。
- **ファイル参照に戻す方法**：GitHub Pagesなど実際のフォルダ構成で公開する場合は、
  `src="data:image/jpeg;base64,..."`の部分を`src="assets/images/ファイル名.jpg"`に
  書き換えれば、通常のファイル参照に戻せます（実際のホスティング環境では
  相対パスは問題なく機能します）。

## セクション構成（index.html内のid、ページに表示される順）

| セクション | id | 内容 |
|---|---|---|
| ヒーロー | `#top` | Teach Together. Leave Earlier. |
| 講師紹介 | `#instructor` | プロフィール・資格バッジ（Base Designと同一内容） |
| 著作紹介 | `#book` | 書影・実績バッジ（Base Designと同一内容） |
| 4つの使い方 | `#usage` | A当日に使う／B自分で学ぶ／C復習する／D今後使う |
| 今日の研修 | `#today` | 研修テーマ＋SESSION 0〜5のカード（開くボタンで全画面表示） |
| SESSION 0 全画面ビュー | `#session0-view` | オープニング（今日のゴール・5つのツール・今日使うリンク） |
| SESSION 1 全画面ビュー | `#session1-view` | 「話すだけ」で校務が終わる時代へ（音声→Gemini） |
| SESSION 2 全画面ビュー | `#session2-view` | Gemini Notebookを先生の共同担任にする（授業案・確認問題） |
| SESSION 3 全画面ビュー | `#session3-view` | Googleフォーム × AI（回答体験→Gemini→Brisk） |
| SESSION 4 全画面ビュー | `#session4-view` | Canva AIで校務をもっと軽くする（PowerPoint→Canva AI） |
| SESSION 5 全画面ビュー | `#session5-view` | 明日から使えるAI活用ロードマップ（LEVEL1〜5） |
| AIツールの使い分け | `#tools` | Gemini／Gemini Notebook／Googleフォーム／Canva／Brisk／ChatGPT／Claude／Copilotの8カード |
| 使い方ガイド／復習コーナー | `#guides` | iPad「ボイスメモ」・Gemini Notebook・Googleフォーム・Brisk・Canva AIの5カード |
| 小学校向け活用例 | `#subjects` | 国語〜管理職・教務まで14の教科・分掌カード |
| AI活用ロードマップ | `#roadmap` | SESSION 5と同内容のLEVEL1〜5（研修後に単独で見返す用） |
| 研修後に復習する | `#after` | クイックリンク・よくある質問（FAQ）7問・参考リンク |
| プロンプトライブラリ | `#prompts` | 音声・校務／授業／学級・保護者の3カテゴリー |
| 明日からやること | `#action` | 5つの選択肢から1つ以上選ぶミニアクションカード |

`#tools`（AIツールの使い分け）は「今日の研修」の直後・「使い方ガイド」の直前に配置しています。
研修本編（SESSION 0〜5）を一通り終えたあと、使ったツール同士の違いを整理してから、
自分のペースで学ぶ使い方ガイドへ進む流れです。

## SESSION 0〜5の全画面ビューについて

ヘッダーメニュー・「今日の研修」カードの「開く」ボタン・各講座末尾の「続けて次の講座へ」
リンクなど、`href="#session0"`〜`href="#session5"` を持つリンクをクリックすると、
その講座だけを覆う全画面ビューが開きます（他の講座が開いていた場合は自動的に閉じ、
常に1つだけが表示されます）。入場時にはサイトの濃いグリーン（`--color-blue-deep`）の
幕が下から上へ流れて消える演出が入ります。ビュー右上の「閉じる」ボタン、またはEscキーで
元の画面に戻れます。この仕組み・演出はBase Designから一切変更していません
（`script.js`の`initSessionViews()`、対象idは`SESSION_VIEW_IDS`で管理）。

## 「明日からやること」選択カードについて（新規追加）

SESSION 5の末尾と、独立したセクション`#action`の2箇所に、同じ5つの選択肢を用意しています。
チェックした数に応じて短いメッセージが表示されますが、**選択内容はサーバーにも
ブラウザにも保存されません**（ページを閉じるとリセットされます）。保存が必要な場合は、
別途アンケートフォーム等と連携する改修が必要です。

## 差し替え・確認が必要な箇所一覧

公開前に、以下を確認・差し替えてください。

- [ ] `<meta name="training-date">`（`index.html`頭部）：研修実施日・時間帯を確定日時に書き換える
- [ ] `#guides`（使い方ガイド／復習コーナー）：動画を追加したい場合は、各カードに
      `<div class="video-frame"><iframe ...></iframe></div>`（`style.css`に定義済みの
      `.video-frame`）を追加してください。現在は動画なしのテキスト＋リンクのみの構成です
- [ ] SESSION 1の演習手順にあるtip-card（デモ音声についての案内）：演習用デモ音声の
      ファイル名・入手方法を記載する
- [ ] 講師紹介（`#instructor`）・著作紹介（`#book`）：Base Designの内容（和田倫周氏）を
      そのまま引き継いでいます。紫原小学校の研修も同じ講師が担当する場合はこのままで
      構いません。別の講師が担当する場合は、氏名・肩書き・経歴と、画像・動画（現在は
      `index.html`内にBase64で埋め込み。差し替え方法は上記「画像・動画について」を参照）を
      差し替えてください
- [ ] フッター・SESSION内フッターの問い合わせ先（`michihirowadarin@gmail.com`）：
      担当が変わる場合は差し替える
- [ ] Session3の「学級アンケート」「授業振り返りフォーム」演習：実際に使う教材・
      対象学年に応じて、プロンプト内の【　】部分を調整する
- [ ] `#subjects`（小学校向け活用例）の各教科の一文：紫原小学校の実態に応じて、
      具体的な活用例に書き足していく
- [ ] SESSION 2の「夏休み明け最初の授業」デモ：実際に使う教材があれば差し替える
- [ ] 各SESSIONの「よくあるトラブル」：実際の研修で出たトラブルがあれば追記する

「不明な画像・URL・プロフィール情報・外部リンク」は上記以外に作成していません。
Gemini・Gemini Notebook（NotebookLM）・Googleフォーム・Brisk・Canva for Educationの
公式サイトURLは、Base Designで使用されていたものをそのまま引き継いでいます。

## GitHub Pagesへアップロードする際のファイル一覧

以下をリポジトリ直下にそのままアップロードしてください（フォルダ構成を保ったまま）。

```
index.html
style.css
script.js
README.md
assets/images/instructor-illustration.jpg
assets/images/google-trainer-badge.jpg
assets/images/canvassador-badge.jpg
assets/images/book-cover.jpg
assets/videos/            （動画を追加する場合はこの中に配置し、.gitkeepは削除して構いません）
```

## プロンプトの追加方法

`#prompts`内の各カテゴリー（音声・校務／授業／学級・保護者）の
`<div class="accordion-panel">`内に、`.prompt-card`ブロックをコピーして追加してください。
`.prompt-body`の`id`は、ページ内で重複しないユニークな値にしてください。`.copy-btn`の
`data-copy-target`に対応する`id`を`#`付きで指定すると、コピー機能が自動的に有効になります。

## 配色の変更方法

`style.css`先頭の`:root { ... }`ブロックの値だけを書き換えれば、サイト全体の色が
変わります（Base Designと同じ変数構成です）。

## 公開前の確認事項

- [ ] 上記「差し替え・確認が必要な箇所一覧」をすべて確認したか
- [ ] すべてのページ内リンク（`#today` `#prompts` など）が正しいセクションへ移動するか
- [ ] Chromebook・iPad・iPhone・AndroidのSafari／Chromeで表示したときに横スクロールが
      発生しないか、ボタンが押しやすいサイズか
- [ ] プロンプトのコピーボタンが動作し、「コピーしました」の通知が表示されるか
- [ ] オープニング演出（濃いグリーンの幕が下から上へ流れて消える）が2秒程度で終わり、
      操作を妨げないか
- [ ] SESSION 0〜5の全画面ビューが、メニューやボタンから正しく開くか。幕の演出のあとに
      内容が表示されるか
- [ ] 各講座末尾の「続けて次の講座へ」リンクで、正しく次の講座へ切り替わるか
- [ ] SESSION全画面ビューの「閉じる」ボタン・Escキーで元の画面に戻れるか
- [ ] 講師写真・書影の画像が正しく表示されるか（読み込めない場合もレイアウトが崩れないか）
- [ ] 講師紹介のアニメーション動画が自動再生・ループするか（自動再生できない環境では
      静止画が代わりに表示されるか）
- [ ] 「明日からやること」のチェックカードが選択状態を切り替えられるか
- [ ] GitHub Pages等で公開後、実機（スマートフォン・iPad・PC）で表示確認したか

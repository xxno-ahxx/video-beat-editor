# 🎬 Beat Sync Editor — 音ハメ動画メーカー
## 🌐 公開サイト

https://xxno-ahxx.github.io/video-beat-editor/

動画素材とCCライセンス楽曲から**音ハメ動画を自動生成**するWebアプリです。
サーバー不要・すべてブラウザ内で完結します（GitHub Pagesでホスティング）。

**公開URL：** https://xxno-ahxx.github.io/video-beat-editor/

## 機能

1. **動画アップロード** — MP4/MOVを複数ドラッグ＆ドロップ
2. **楽曲検索・試聴・選択** — [Jamendo API](https://www.jamendo.com/)（クリエイティブ・コモンズ楽曲）
3. **テロップ設定** — クリップごとにテキストを設定（フェードイン表示）
4. **自動編集** — Web Audio APIでビート検出し、ビートに合わせてクリップを自動切替。切替時にビープ効果音、BGMを全体に重ねる
5. **書き出し** — Canvas + MediaRecorderで録画し、FFmpeg.wasmでMP4に変換してダウンロード

## 使い方

1. 公開URLをChromeなどのモダンブラウザで開く
2. ①動画をアップロード → ②曲を検索して選択 → ③カット間隔などを設定 → ④プレビュー確認 → MP4書き出し

### Jamendo APIキーについて

- デフォルトでデモキーが入っています（リクエスト回数制限あり）
- 本格的に使う場合は https://devportal.jamendo.com/ で無料キーを取得し、画面上部の入力欄に貼り付けて「キー保存」

## 技術スタック

| 要素 | 技術 |
|------|------|
| フロントエンド | HTML + CSS + Vanilla JS（`index.html` 単一ファイル） |
| ビート検出 | Web Audio API（エネルギーベースのピーク検出） |
| 動画合成 | Canvas 2D + MediaRecorder（webm録画） |
| MP4変換 | FFmpeg.wasm（シングルスレッド版コア・CDN読み込み） |
| 楽曲API | Jamendo API v3.0 |
| ホスティング | GitHub Pages（GitHub Actionsで自動デプロイ） |

### 設計メモ

- GitHub PagesはCOOP/COEPヘッダーを設定できないため、SharedArrayBufferが不要な**シングルスレッド版FFmpegコア**を使用
- 編集処理はFFmpegではなく**Canvasリアルタイム合成→録画**方式（wasmでの多入力フィルタ処理よりメモリ効率・安定性が高い）
- SafariなどMP4直録画に対応したブラウザではFFmpeg変換をスキップ
- Jamendo音源がCORS等で取得できない環境向けに、手持ち音源ファイルのアップロードにも対応

## ライセンス・注意事項

- 本ツールは**非商用利用**を想定しています
- Jamendoの楽曲はトラックごとにCCライセンス条件（クレジット表記・改変可否など）が異なります。動画公開時は各トラックのライセンス表示に従ってください
- アップロードした動画・音源が外部サーバーに送信されることはありません（全処理がブラウザ内）

# kofa-excel-format（Excelフォーマット自動作成）

Excelテンプレート（.xlsx）に `{{プレースホルダ}}` を置いておくと、対戦カードの情報を
差し込んで複数のExcelを一括出力するWebアプリ。
公開URL: https://sagerbafcsec4.github.io/kofa-excel-format/

すべてブラウザ内で処理し、ファイルは外部送信されません。

## 修正する人へ（重要）

- **画面とロジックは `index.html` の中に全部入っています**（唯一の正本）。
- 修正は **`index.html` を直して、このリポジトリにコミットするだけ**。数十秒で公開ページに反映。
- PWA（オフライン対応）なので更新後に古い表示が残ることがあります → **Ctrl+F5**（スマホは再読み込み）。
- 別デバイス・別のClaude/Coworkでも、**まずこの README と `index.html` を読めば把握できます。**

## 何をするアプリか

1. ベースとなる **Excelテンプレート（.xlsx）** を読み込む。テンプレ内に `{{HOME_TEAM}}` 等の
   プレースホルダを書いておく。
2. 対戦カード（ホーム/アウェイ）や大会・節・日付などを入力（チーム表・スタジアム対応・CSV取込あり）。
3. プレースホルダを置換して、試合ごとに .xlsx を生成。フォルダ保存またはZIP一括保存。

### 使えるプレースホルダ
`{{HOME_TEAM}}` `{{AWAY_TEAM}}` `{{HOME_STADIUM}}` `{{AWAY_STADIUM}}`
`{{HOME_CITY}}` `{{AWAY_CITY}}` `{{HOME_COUNTRY}}` `{{AWAY_COUNTRY}}`
`{{COMPETITION}}` `{{SEASON}}` `{{MATCHDAY}}` `{{KICKOFF}}` `{{DATE}}` `{{DATE8}}`

## 技術メモ

- 外部ライブラリは **JSZip** のみ（.xlsx は実体がZIPなので、内部XMLの文字列を直接置換している。SheetJSは不使用）。
- 入力したチーム情報などは **ブラウザの localStorage** に保存（`load`/`save`）。
- 主な関数: `fillTemplate`（プレースホルダ置換）/ `buildAll`（全試合生成）/ `renderRows`（対戦リスト）/
  `importCSV`・`dlCsvTemplate`（CSV）/ `saveToFolder`・`dl`（保存）/ `stadFor`（スタジアム照合）。

## ファイル

| ファイル | 役割 |
|---|---|
| `index.html` | アプリ本体（画面＋ロジック。**唯一の正本**） |
| `manifest.webmanifest` / `sw.js` | PWA用（ホーム追加・オフライン） |
| `icon-*.png` | アイコン |

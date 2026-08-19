# kofa-excel-format（Excelフォーマット自動作成）

Excelテンプレート（.xlsx）に `{{プレースホルダ}}` を置いておくと、対戦カードの情報を
差し込んで複数のExcelを一括出力するWebアプリ。

- **本番公開URL（Cloudflare Pages・2026-08-19〜）**: https://kofa-excel-format.pages.dev/
- 旧公開URL（GitHub Pages・放置中／リンク切れ防止のためアクセスは可能）: https://sagerbafcsec4.github.io/kofa-excel-format/

すべてブラウザ内で処理し、ファイルは外部送信されません。

## 修正する人へ（重要）

- **画面とロジックは `index.html` の中に全部入っています**（唯一の正本）。
- 修正は **`index.html` を直して、このリポジトリにコミット・push**（GitHub Pagesは自動反映されるが、旧URLなので実質不要）。
- **本番のCloudflare Pagesは自動反映されない**（GitHub連携ではなく手動アップロードのため。他のkofaアプリと同じ方式）。反映するには以下を実行:
  ```bash
  # index.html / manifest.webmanifest / sw.js / icon-*.png を専用フォルダにコピーしてから
  npx wrangler pages deploy <そのフォルダ> --project-name=kofa-excel-format
  ```
  初回のみ `npx wrangler login` でログインが必要（このマシンでは既にログイン済み）。
- PWA（オフライン対応）なので更新後に古い表示が残ることがあります → **Ctrl+F5**（スマホは再読み込み）。
- 別デバイス・別のClaude/Coworkでも、**まずこの README と `index.html` を読めば把握できます。**

## 何をするアプリか

1. ベースとなる **Excelテンプレート（.xlsx）** を読み込む。テンプレ内に `{{HOME_TEAM}}` 等の
   プレースホルダを書いておく。
2. 対戦カード（ホーム/アウェイ）や大会・節・日付などを入力（チーム表・スタジアム対応・CSV取込あり）。
3. プレースホルダを置換して、試合ごとに .xlsx を生成。フォルダ保存またはZIP一括保存。

### 共通設定・対戦リストの入力（2026-08-19〜）

- **シーズン・大会名はプルダウン選択**。大会名は「プレミアリーグ／ラ・リーガ／エールディヴィジ／カスタム」の4択（`LEAGUE_OPTIONS` 定数）。カスタムを選ぶと自由入力欄に切り替わる。
- **対戦リストのホーム/アウェイもプルダウン選択**。共通設定で選んだ大会名に紐づく「所属リーグ」のチームだけが選択肢に出る（大会名が未選択のときは「先に大会名を選んでください」と表示、カスタムのときは自由入力に戻る）。
- チームの所属リーグは**チーム一覧の共有スプレッドシートの「所属リーグ」列**（チーム名の右隣）で管理する。値は上記4択のうち「プレミアリーグ／ラ・リーガ／エールディヴィジ」のいずれかを書く（空欄のチームはカスタム大会でのみ選べる）。
- 「まとめて貼り付け」で入力したチーム名は、選択中の大会の登録チーム名（または別表記）と完全一致した場合だけ自動選択される。一致しない名前は空欄になり、件数と該当名を通知（トースト＋アラート）。

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

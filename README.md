# 墨田区 保活シミュレーター

墨田区の利用調整基準（基準指数・調整指数・優先順位）で世帯の合計指数を計算し、
過去の4月一次利用調整結果（令和3〜8年）と照らして、各認可保育施設の0歳・1歳クラスの入園の見込みを表示する非公式ツールです。
フロントエンドのみ（`public/index.html` 1ファイル）で、入力内容はブラウザの外に送信しません。

## 構成

```
public/index.html   ページ本体（HTML/CSS/JS すべて含む）
wrangler.jsonc      Cloudflare Workers（静的アセット）の設定
package.json        wrangler のスクリプト
```

## デプロイ

### A. GitHub 連携（Workers Builds）
1. このフォルダをそのまま GitHub のリポジトリに push する
2. Cloudflare ダッシュボード → Workers & Pages → 作成 → 「リポジトリをインポート」でそのリポジトリを選ぶ
3. ビルドコマンドは空のまま、デプロイコマンドは `npx wrangler deploy`
4. 以降は main に push するたびに自動でデプロイされる

### B. 手元からデプロイ
```
npm install
npx wrangler login
npm run deploy
```
ローカル確認は `npm run dev`。

## データの出典（すべて墨田区公式）
- 令和8年用 認可保育施設利用申込みのご案内（利用調整基準 P25〜27、施設一覧 P17〜22）
  https://www.city.sumida.lg.jp/kosodate_kyouiku/kosodate_site/azukeru/hoikuen/nyuuen/riyou_annai/a.files/R8goannai.pdf
- 4月入園の第一次利用調整結果（入所下限合計指数）各年
  https://www.city.sumida.lg.jp/kosodate_kyouiku/kosodate_site/azukeru/hoikuen/nyuuen/shinsei_joukyou/ta80320020170821.html

## データの更新
基準・園・結果のデータは `public/index.html` の `<script>` 内に直接書いています。
- `BASE` / `ADJ` / `SINGLE` / `WELFARE` / `PRI`：利用調整基準
- `DATA`：施設一覧と最新年（令和8年）の0歳クラス下限
- `HIST`：過去年（令和7〜3年）の0歳クラス下限
- `ONE` / `ONLY1`：1歳クラスの定員と令和8〜3年の下限（`ONLY1` は0歳クラスがない園）
- `CHO`：町丁目の位置（距離の並び替え用の概算。区の資料ではない）

新しい年の結果が出たら、`DATA` の下限を更新し、前年分を `HIST` の先頭に追加、`YEARS` / `YLABEL` に年を足してください。

## 注意
- 墨田区の公式ツールではありません。結果は過去の実績に基づく目安で、入園を保証するものではありません。
- このシミュレーターの結果と、実際の入園内定の結果は異なることがあります。
- 過去のPDFの0歳クラス欄は、PDFの文字から読み取った値です。申込み前に区の原本で確認してください。
- 「R1-2」などの表記は年度区分として解釈しています。

# みんなの歴史文化マップ(旧:Open History & Culture Map)
## このアプリについて
- OpenStreetMapのデータから歴史、文化関連の地物を表示するWebアプリです。
- 以下のサイトで公開しています。どなたでもご自由にご利用ください。
- https://armd-02.github.io/openhc-map/

## fork元のプロジェクト
- Open History & Culture MapはCommunity Mapmakerをベースに開発しています。
- Open History & Culture Mapの成果はCommunity Mapmakerへ還元する予定です。
- https://github.com/K-Sakanoshita/community_mapmaker

## 活動データの接続

活動データは [Community Mapmaker Backend](https://github.com/K-Sakanoshita/community_mapmaker_backend) のプロジェクト `openhc` から取得します。`data/config-user.jsonc` の設定は次のとおりです。

```json
"activity": {
    "url": "https://armd-01.sakura.ne.jp/community-mapmaker-backend/api/activities.php?app=openhc",
    "authMode": "basic",
    "targetName": "activity"
}
```

閲覧時は公開APIを使用し、投稿・編集時はバックエンドの認証を使用します。既存の活動ID（`openhc-map/...`）とフォーム設定は引き継いでいます。旧 `google.AppScript` の値は保存していますが、接続には `activity.url` を使用します。

## ローカルでの確認

ビルドは不要です。リポジトリのルートでHTTPサーバーを起動します。

```bash
python3 -m http.server 8765 --bind 127.0.0.1
```

[ローカルの地図](http://127.0.0.1:8765/)を開いてください。回帰テストは `node tests/run.cjs` で実行します。

本番バックエンドから活動データを取得するには、バックエンドの `auth.allowed_origins` に `http://127.0.0.1:8765` を追加する必要があります。Originはプロトコル・ホスト名・ポート番号の組み合わせなので、`localhost` や別のポートで開く場合は、そのOriginも個別に許可してください。

許可されていない接続元には `403 origin_not_allowed` が返ります。この場合、OSMの地物が表示されていても、バックエンドの説明文・確認日・写真などの活動情報は表示されません。APIのURLを直接開けても、地図からの取得が許可されているとは限りません。

公開サイトのOriginは `https://armd-02.github.io` です。バックエンドの許可設定にはリポジトリのパス `/openhc-map/` を含めません。

## 更新履歴
- 2022/03/04 初版公開
- 2022/03/05 アート作品も表示するように機能追加
- 2022/03/10 敷地を地図上に表示するよう機能追加
- 2022/04/13 以下の機能追加
  - アイコンと種類名を色々と追加
  - Wikipedia記事に画像を表示する機能を追加
  - カテゴリ選択をURLに含める機能を追加
- 2022/09/08 以下の機能追加
  - コミュニティマップメーカーのベースバージョン更新
  - 地図の初期表示を京都・御所へ変更
  - 起動時のスプラッシュ画像を更新
  - 表示されるアイコン種別を色々と追加
  - 案内板をカテゴリ選択できるよう追加
  - facebookからリンクを開けない不具合を修正
- 2022/10/02 以下の機能追加
  - 若干の表示に至るまでの待ち時間を改善
  - 表示されるアイコン種別を色々と追加
  - ズームレベルで表示する地物を細分化(historicをエリアとPOIに分割)
  - 一部表示されない地物が表示されるようになる
- 2022/11/01 小さなバグを修正
- 2022/11/05 小さなバグを修正&ライブラリの更新
- 2025/01/16 大規模システム更新
  - タイトル変更
    - 「Open History & Culture Map」から「みんなの歴史文化マップ」へ
  - ベースシステムを最新のCommunity Mapmakerへ更新
  - アイコン追加（歴史的建築物）
  - 建物とアイコン取得の処理でbuilding=yesの優先順位を下げる
- 2025/02/03 Google アナリティクスを設定
- 2026/11/03 [Community Map Maker最新版](https://github.com/K-Sakanoshita/community_mapmaker/commit/8526f8c62c310871a744d88c7f1b181be2d98e7f)へ更新
  - 歴史文化マップのカテゴリ・初期位置などの既存設定を引き継ぎ、設定形式とPNGアイコンを移行
  - 活動データの接続先をバックエンドのプロジェクト `openhc` に変更
  - ローカル起動手順とバックエンドのOrigin許可設定を追記
  - メニューとリストの背景を淡い和紙に変更
  - リストの縦折り返しと名前のない地物のアイコン欠落を修正し、宗教・宗派・年代を補足表示
  - 詳細が短い地物でもOpenStreetMapのメニューが隠れないよう修正
  - 道しるべ・観光案内所・情報端末を取得対象から除外（案内図は継続表示）
  - 取得できないサムネイルを非表示にし、対象URL・地物ID・失敗理由をコンソールに出力

- 2026/11/04 スプラッシュ・シェア画像を刷新
  - 「まちの歴史と文化を、みんなで地図に残す。」をテーマに、手書き文字と実際の地図を採用
  - シェア用の説明文・画像サイズ・画像の代替テキストを整備

- 2026/11/05 背景地図の表示名を分かりやすく変更
  - OpenStreetMapの3D・標準地図、地理院の淡色地図・航空写真を区別できる表記に変更

- 2026/11/07 歴史カテゴリの名称と設定を更新
  - `historic=manor`（邸宅）と `historic=stone`（歴史的な石）の日本語・英語の表示名を追加
  - 優先するOverpass接続先を `https://armd-01.sakura.ne.jp/api/interpreter` に変更
  - 背景マーカーの設定から `circle_marker.png` を除外
  - 不要な画像・画像編集元とVS Codeのローカル設定を削除

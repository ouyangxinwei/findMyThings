# Find My Things

日本国内の指定地点から、指定した種類の施設・店舗を検索する小さなWebアプリです。

## MVP

- GPSで現在地を取得
- 郵便番号から住所を取得（ポストくん）
- 住所から緯度・経度を取得（国土地理院）
- OpenStreetMap / Overpass APIで選択した種類の施設・店舗を検索
- 直線距離を計算し、近い順に表示
- 東京駅の固定デモ位置でGPSなしでも検索を試せる

## 検索できる種類

画面の「検索する種類」から、次のカテゴリを1つ選んで検索できます。

| カテゴリ | OpenStreetMapの主なタグ |
| --- | --- |
| コンビニ | `shop=convenience` |
| スーパー | `shop=supermarket` |
| 物流会社 | `office=logistics` |
| 貨物・運送会社 | `office=transport` |
| 宅配・配送会社 | `office=courier` |
| 郵便・荷物の配送センター | `amenity=post_depot` |
| 郵便局 | `amenity=post_office` |

物流会社のオフィス、貨物輸送会社、一般向けの宅配窓口、郵便物の配送センターは別カテゴリとして扱います。配送センターは一般利用者向けの窓口がない施設も含みます。

## 起動方法

### ローカル

1. このリポジトリを clone またはダウンロード
2. `index.html` をローカルWebサーバーで開く（VS Code Live Server等）
3. GPSを使う場合は HTTPS または localhost で開く

GitHub Pagesでも公開できます。GPSの利用にはブラウザの位置情報許可が必要です。

## 使用サービス

| 用途 | サービス | APIキー |
| --- | --- | --- |
| 郵便番号 → 住所 | [ポストくん](https://postcode.teraren.com/doc/) | 不要 |
| 住所 → 座標 | [国土地理院 住所検索](https://msearch.gsi.go.jp/address-search/AddressSearch) | 不要 |
| 周辺店舗検索 | [Overpass API](https://overpass-api.de/) / OpenStreetMap | 不要 |

## 注意事項

- 検索結果は選択カテゴリに対応するOpenStreetMapタグで登録された情報です。タグ付けの有無や精度に左右されるため、すべての施設・企業が網羅されるわけではありません。特に物流関連施設は、地図上に登録されていない場合や別のタグで登録されている場合があります。
- 距離は道路距離・徒歩時間ではなく、緯度経度から計算した直線距離です。
- Overpass APIは共有サービスです。短時間に大量の検索を繰り返さないでください。障害や混雑時は時間をおいて再試行してください。
- 店舗検索はOverpass APIの接続先を順番に試し、各接続先で最大45秒待機します。5km検索はデータ量が増えるため、混雑時は検索に時間がかかったり失敗したりする場合があります。
- 郵便番号APIや住所検索APIが応答しない場合、その入力方法は一時的に利用できないことがあります。GPSまたはデモ位置を利用できます。
- デモ位置は東京駅の固定座標であり、現在地ではありません。
- このMVPでは地図表示、ログイン、検索履歴、店舗営業時間、経路案内、サーバーは実装していません。

## データ帰属

© OpenStreetMap contributors. OpenStreetMapデータはODbLライセンスに基づきます。詳細は https://www.openstreetmap.org/copyright を参照してください。

## バージョン

現在のバージョンは [version.json](./version.json) を参照してください。

## GitHub Pages 初回設定

初回のみ、リポジトリの **Settings → Pages → Build and deployment → Source** で **GitHub Actions** を選択してください。その後、`main` への push ごとに自動デプロイされます。

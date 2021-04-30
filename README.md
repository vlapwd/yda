## YouTube Data API

`https : / /www.googleapis.com/youtube/v3/channels?part=statistics&id=[チャンネルID]`
でチャンネル登録者数とかがとれる。ただ端数(1000単位？)が切り捨てられてそう  
チャンネルIDは`https : //youtube.com/channnel/[これ]`

```json
{
  "kind": "youtube#channelListResponse",
  "etag": "223yUqUGpRx5spfztD5G9mXpEcs",
  "pageInfo": {
    "totalResults": 1,
    "resultsPerPage": 5
  },
  "items": [
    {
      "kind": "youtube#channel",
      "etag": "jyvcZ0uWO7wGNY7B5ajgkD8nAWg",
      "id": "UCkIimWZ9gBJRamKF0rmPU8w",
      "statistics": {
        "viewCount": "26420221",
        "subscriberCount": "273000",
        "hiddenSubscriberCount": false,
        "videoCount": "468"
      }
    }
  ]
}
```

GCPのダッシュボードの、APIとサービス > ライブラリ > Youtube Data API v3 で有効にするを選択  
そのまま、管理 > 認証情報の認証情報を作成でOAuth2.0クライアントIDを作成  
アプリケーションの種類はデスクトップアプリを選択  
(ウェブアプリケーションを選びたくなるが、この後でダウンロードするclient_secret.jsonにredirect_urisが作成されないため(必須))  
作成できたら作ったクライアントIDのJSONをダウンロードからclient_secret.jsonをダウンロードし、プロジェクトの良き場所へ  
[クイックスタート](https://github.com/youtube/api-samples/blob/master/go/quickstart.go)がある  
[forUserName](https://github.com/youtube/api-samples/blob/master/go/quickstart.go#L109)となっているが、チャンネルIDから所得したいので[Id](https://pkg.go.dev/google.golang.org/api/youtube/v3#ChannelsListCall.Id)に変更  
[レスポンスの中身](https://github.com/googleapis/google-api-go-client/blob/de9f25cba16a59e6a62c6b97478882f5392143f5/youtube/v3/youtube-gen.go#L1925)  
```go
fmt.Println(fmt.Sprintf("%s: チャンネル登録者数 %v",
	response.Items[0].Snippet.Title,
	response.Items[0].Statistics.SubscriberCount,
))
```
```
% go run main.go
// 天宮 こころ / Amamya Ch.: チャンネル登録者数 273000
```

参考  
[公式](https://developers.google.com/youtube/v3/getting-started?hl=ja)  
[google api go client](https://github.com/googleapis/google-api-go-client/tree/master/youtube/v3)  
[api サンプル](https://github.com/youtube/api-samples)  
[pkg.go.dev](https://pkg.go.dev/google.golang.org/api/youtube/v3)
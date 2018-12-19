# 99900_Response

<!--aside class="notice">This response section is stored in a separate file in `includes/_response.md`. Whiteboard allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside-->

## 99901_Headers
Name | Description
---------- | -------
ETag | キャッシュ用ハッシュ値。Response Bodyの内容をmd5化した文字列が返る。

## 99902_HTTP Status Code

### 正常系

Status Code | Description
---------- | -------
200 - OK | GET系リクエストの正常系
201 - Created | POST,PUT系リクエストの正常系
204 - No Content | DELETE系リクエストの正常系

### 異常系

Status Code | Title | Description
---------- | ------- | --------
303 - SeeOther | 初回設定エラー | 初回設定が未設定だった場合。
307 - TemporaryRedirect | 時間外エラー | 取引/出金の時間外等、一時的にアクセスを拒否したい時。
400 - BadRequestError | 汎用エラー | リクエストパラメータに異常がある時。
401 - Unauthorized | 認証エラー | トークン消失/有効期限切れの時。
403 - Forbidden | 汎用エラー | 許可がないコンテンツににアクセスした時。
404 - Not Found | 汎用エラー | アクセスしたリソースが見つからない時。
405 - MethodNotAllowedError | 汎用エラー | エンドポイントに対して定義されていないMethodにてアクセスがあった時。
409 - Conflict | バリデーションエラー | すでに存在しているに対してPOSTされた時。`rules` に対象フィールドとそのエラー内容が返る。
422 - UnprocessableEntity | バリデーションエラー | POSTパラメータに異常がある時。`rules` に対象フィールドとそのエラー内容が返る。
500 - InternalServerError | 汎用エラー | サーバーロジック等に問題がある時。
503 - ServiceUnavailableError | サービス利用不可エラー | メンテナンス等。`message` にその理由が返る。
505 - ClientVersionNotSupportedError | 強制アップデートエラー | クライアントバージョンがサーバーの許容外の時。 `message` にアップデートページへのURLが返る

### 異常系(Response Body)

Attributes | Type | Description
---------- | ------| -------
type | string | エラータイプ
message | string | エラー内容
rules | array:object | バリデーションエラー内容

```bash
#303 初回設定エラー
{
  "type": "SeeOther",
  "message": "tutorial:0",
  "rules": {}
}

#307 時間外エラー
{
  "type": "TemporaryRedirect",
  "message": "You can't access to this feature at this time.\r\nOrders available 24 hours except for Sunday (Deposit is available anytime)\r\n* We will reopen these features once hard fork is over.",
  "rules": {}
}

#401 認証エラー
{
  "type": "UnauthorizedError",
  "message": "Authentification Failed",
  "rules":{}
}

#422 バリデーションエラー
##リクエストパラメータが不正だった場合
{
  "type": "InvalidRequestError",
  "message": "",
  "rules":{
    "content": ["投稿内容は入力必須です"]
  }
}

##リクエストパラメータに関係ないバリデーションエラー(ユーザーデータ不整合等)の場合
{
  "type": "InvalidRequestError",
  "message": "",
  "rules":{ 
	  "_msg": ["既にチップを送っています"] 
  }
}

#503 サービス利用不可エラー
##メンテナンス中
{
  "type": "ServiceUnavailableError",
  "message": "Under system maintenance",
  "rules": {}
}

#505 強制アップデートエラー
{
  "type": "ClientVersionNotSupportedError",
  "message": "https://itunes.apple.com/app/apple-store/id1401257503",
  "rules": {}
}

```

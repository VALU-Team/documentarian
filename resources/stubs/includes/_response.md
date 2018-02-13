# 999_Response

<!--aside class="notice">This response section is stored in a separate file in `includes/_response.md`. Whiteboard allows you to optionally separate out your docs into many files...just save them to the `includes` folder and add them to the top of your `index.md`'s frontmatter. Files are included in the order listed.</aside-->

## Headers
Name | Description
---------- | -------
ETag | キャッシュ用ハッシュ値。Response Bodyの内容をmd5化した文字列が返る。

## HTTP Status Code

### 正常系

Status Code | Description
---------- | -------
200 - OK | GET系リクエストの正常系
201 - Created | POST,PUT系リクエストの正常系
204 - No Content | DELETE系リクエストの正常系

### 異常系

Status Code | Description
---------- | -------
401 - Unauthorized | 認証トークンエラー
403 - Forbidden | アクセス不許可エラー
404 - Not Found | リソースが見つからない時のエラー
409 - Conflict | リソースコンフリクトエラー。`rules` に対象フィールドとそのエラー内容が返る。
422 - Unprocessable Entity | バリデーションエラー。`rules` に対象フィールドとそのエラー内容が返る。
500 - Internal Server Error | システムエラー
503 - Service Unavailable Error | サービス利用不可エラー。`message` にその理由が返る。

### 異常系(Response Body)

Attributes | Type | Description
---------- | ------| -------
type | string | エラータイプ
message | string | エラー内容
rules | array:object | バリデーションエラー内容

```bash
#422 Error Handling
##リクエストパラメータがバリデーションエラーの場合
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

#503 Error Handling
##メンテナンス中
{
  "type": "ServiceUnavailableError",
  "message": "Under system maintenance",
  "rules": {}
}

##営業時間外(Wallet, Order機能限定)
{
  "type": "ServiceUnavailableError",
  "message": "売買取引は日曜日を除く9:00~21:00。出金の受付は、土日祝日を除く平日の 9:00 ～ 21:00 となっております。",
  "rules": {}
}
```

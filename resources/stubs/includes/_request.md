#99800_Request

## 99801_Headers
Name | Value | Description
---------- | -------
Authorization: | Bearer ${api_token} | ユーザー認証用アクセストークン
Content-Type: | multipart/form-data | POST, PUT時のみ使用
X-Client-Lang: | (en\|jp) | デバイス言語設定 ※1
X-Client-Name: | (ios_app\|android_app) | クライアント名 
X-Client-Version: | x.y.z | クライアントバージョン番号

※1) 1.DBの言語 2.リクエストパラメータ(lang)の言語 3.X-ClientLangの言語 の順で参照される

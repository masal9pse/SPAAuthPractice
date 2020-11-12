## メモ

config のキャッシュをクリア

```
rm -f bootstrap/cache/config.php
```

実行した情報を cookie.txt に保存する

```
curl -c cookie.txt -X POST localhost:8000/api/login -d email=hoge@example.com -d password=hogehoge
```

# 社畜丼ワードクラウド

ローカルタイムラインから一時間ぶんの投稿を取得し、ワードクラウドを作成、投稿します。

### プログラムを見る・試す

1. ワードクラウドを生成する元になる画像を、 `background.jpg` として同一ディレクトリに準備する。
2. `timeline.py` の6行目のMastodonのインスタンスURLを適切に書き換える
3. MastodonのWebの設定→開発画面でアプリケーションの登録を行う
4. `my_access_token.txt` , `my_client_key.txt` にそれぞれアクセストークンとクライアントキーをコピペする
5. 以下のコマンドを実行する
```bash
$ git clone https://github.com/mstdn-workers/wordcloud.git
$ cd wordcloud
$ docker build -t wordcloud .
$ docker run -d -p "8888:8888" -v "$PWD:/work/" wordcloud
```

http://localhost:8888 にアクセスします

### インストール
systemdのユーザ権限でのUnitとして定期実行する場合

```bash
$ cp wordcloud.service wordcloud.timer ~/.config/systemd/user/
$ systemctl --user daemon-reload
$ systemctl --user start wordcloud.timer
$ systemctl --user enable wordcloud.timer
```

### コマンドライン引数

```
usage: wordcloud_auto.py [-h]
                         [--since-hour SINCE_HOUR | --range SINCE_HOUR UNTIL_HOUR]
                         [--db DATABASE] [--slow] [--post] [--message MESSAGE]
```

### since-hour , range

```
-h
ヘルプを表示

--since-hour 10
システム日付の 10時〜11時までのトゥートからトレンドを生成

--range 0 8
システム日付の 0時〜8時までのトゥートからトレンドを生成

--post
トゥートをポストする。つけないと内部処理のみを実行（画像は生成されるがトゥートされない）

--slow
画像を使わないトレンドをトゥートする
```

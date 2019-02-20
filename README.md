# AtCoderUsersAPI

## Description

[AtCoder](https://beta.atcoder.jp/)に登録されているユーザーに関する情報を取得できる非公式APIです。

## Features

- usernameおよびTwitterIDで検索
- RESTfulなAPI設計

## Getting Started

```bash
$ curl https://us-central1-atcoderusersapi.cloudfunctions.net/api/info/username/tourist

$ curl https://us-central1-atcoderusersapi.cloudfunctions.net/api/info/TwitterID/que_tourist
```

もしくはブラウザ上で[ここをクリック](https://us-central1-atcoderusersapi.cloudfunctions.net/api/info/username/tourist)

## Usage

### URL

- `https://us-central1-atcoderusersapi.cloudfunctions.net/api/info/[query]/[name]`

    - query:

        - `username`: usernameで検索
        - `TwitterID`: TwitterIDで検索

    - name: 欲しい名前

- `https://us-central1-atcoderusersapi.cloudfunctions.net/api/all/[query]`

    - query:

        - `username`: usernameで検索
        - `TwitterID`: TwitterIDで検索

    - param:

        - `rank`, `birth_year`, `rating`, `highest_rating`, `competitions`, `wins`: start, end を指定可能
        - `country`, `formal_country_name`, `crown`, `user_color`, `affiliation`: 値を指定可能
        - 複数のパラメータ指定は不可
        - 例
        ```bash
        $ curl http://us-central1-atcoderusersapi.cloudfunctions.net/api/all/username?rating[start]=4000&rating[end]=4400

        $ curl http://us-central1-atcoderusersapi.cloudfunctions.net/api/all/username?country=BY

        $ curl http://us-central1-atcoderusersapi.cloudfunctions.net/api/all/username?rating[start]=4000&rating[end]=4400&country=BY
        /* this does not work */
        ```

### Return

`Content-Type:application/json`

- status: Success

```json
/* info/username */
{
    "data":{
        "affiliation": "ITMO University",
        "birth_year": 1994,
        "competitions": 25,
        "country": "BY",
        "crown": "crown4000",
        "formal_country_name": "Belarus",
        "highest_rating": 4208,
        "rank": 1,
        "rating": 4149,
        "twitter_id": "que_tourist",
        "updated": "2018-11-12 11:30:20.272469",
        "user_color": "red",
        "wins": 13
    }
}
```

```json
/* info/TwitterID */
{
    "data":{
        "affiliation": "ITMO University",
        "birth_year": 1994,
        "competitions": 25,
        "country": "BY",
        "crown": "crown4000",
        "formal_country_name": "Belarus",
        "highest_rating": 4208,
        "rank": 1,
        "rating": 4149,
        "updated": "2018-11-12 11:30:20.272469",
        "user_color": "red",
        "username": "tourist",
        "wins": 13
    }
}
```

```json
/* all/username */
{
    "data":{
        "tourist": {
            "affiliation": "ITMO University",
            "birth_year": 1994,
            "competitions": 25,
            "country": "BY",
            "crown": "crown4000",
            "formal_country_name": "Belarus",
            "highest_rating": 4208,
            "rank": 1,
            "rating": 4149,
            "twitter_id": "que_tourist",
            "updated": "2018-11-12 11:30:20.272469",
            "user_color": "red",
            "wins": 13
        }
    }
}
```

- status: Error

```json
{
    "error": {
        "status": 404,
        "title": "Not Found",
        "detail": "info"
    }
}
```

## More Details

- DBは[AtCoderUsersAPI_DB](https://github.com/miozune/AtCoderUsersAPI_DB)で、サーバーレスポンスは[AtCoderUsersAPI_server](https://github.com/miozune/AtCoderUsersAPI_server)で処理しています。
- 5秒インターバルでスクレイピングしているので全ユーザーのクロールに30時間以上かかります。

> まぁ５秒に１回とかなら影響ないとおもいます！ただみんながやり始めるとちょっと変わってくるとは思うので、ちょこちょこ基準が変わるのはご了承ください＞＜
>
> &mdash; chokudai(高橋 直大) (@chokudai) [2018年11月3日](https://twitter.com/chokudai/status/1058526569537269760)

- コンテストが開催され利用者の多い土日は、負荷軽減のためスクレイピングを停止します。(その他必要に応じて手動で停止するかも)

## Author

[@miozune](https://twitter.com/miozune)
# fake-data

fake-data 是一款 http api 服務, 提供假資料回傳、暫存及 NoSQL 儲存的 REST API, 方便各種需要 http 串接的應用開發過程中測試


## Quick Start

```sh
docker run --name fake-data -p 8080:8080 -p 27017:27017 -it --rm vulcanshen/fake-data
```

- echo api: `http://locahost:8080/echo`
- cache api: `http://localhost:8080/cache/{resourceName}`
- keep api: `http://localhost:8080/keep/{resourceName}`
- swagger ui: `http://localhost:8080/openapi/ui`


## Echo API

path: `/echo`

將期望得到的回傳資訊使用 urlencoding 編碼後帶在 query parameter: `feedback` 中發起請求

fake-data 會依照所設定的內容回傳。

feedback 內容格式:

```json
{
   "contentType": "string", // 設定 response 的 header: content-type 屬性，預設為 "application/json"
   "headers": {}, // 設定其他 response headers，預設為空物件
   "status": 0, // 設定 response status code，預設為 200
   "body": {} // 設定 response body 內容，預設為空物件
}
```

範例:

期望 response:

- content-type: text/plain
- status: 201
- headers: 如下
- body: 如下

```json
{
    "contentType": "text/plain",
    "status": 201,
    "headers": {
        "header-key": "header-value"
    },
    "body": {
        "data" : {
            "string-key": "some-string",
            "int-key": 100,
            "array-key": ["1", 2, 3.3]
        }
    }
}
```

將以上 json 使用 urlencoding 編碼得到

```plain
%7B%0A%20%20%20%20%22contentType%22%3A%20%22text%2Fplain%22%2C%0A%20%20%20%20%22status%22%3A%20201%2C%0A%20%20%20%20%22headers%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22header-key%22%3A%20%22header-value%22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%22body%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22data%22%20%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22string-key%22%3A%20%22some-string%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22int-key%22%3A%20100%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22array-key%22%3A%20%5B%221%22%2C%202%2C%203.3%5D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D
```

發起請求:

```sh
curl -v localhost:8080/echo?feedback=%7B%0A%20%20%20%20%22contentType%22%3A%20%22text%2Fplain%22%2C%0A%20%20%20%20%22status%22%3A%20201%2C%0A%20%20%20%20%22headers%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22header-key%22%3A%20%22header-value%22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%22body%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22data%22%20%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22string-key%22%3A%20%22some-string%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22int-key%22%3A%20100%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22array-key%22%3A%20%5B%221%22%2C%202%2C%203.3%5D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D
```

echo API 支援 `GET`, `POST`, `PUT`, `DELETE` 方法

## Cache API

path: `/cache/{resourceName}`

模擬 REST API 的 CRUD 操作

資料會儲存在記憶體中

依照 path 中的 `resourceName` 區分不同資源


REST 操作:

- 新增資料: `POST /cache/{resourceName}`
   - body: 任意 json 物件
- 取得資源清單: `GET /cache/{resourceName}`
- 取得單一資源: `GET /cache/{resourceName}/{id}`
   - id: 資源 id
- 更新資料: `PUT /cache/{resourceName}/{id}`
   - body: 任意 json 物件
- 刪除資料: `DELETE /cache/{resourceName}/{id}`

## Keep API

模擬 REST API 的 CRUD 操作

資料會儲存在容器內的 mongodb 資料庫中

mongodb 對外開啟 27017 port, 可以使用其他 client 端軟體連線

REST 操作:

- 新增資料: `POST /keep/{resourceName}`
   - body: 任意 json 物件
- 取得資源清單: `GET /keep/{resourceName}`
- 取得單一資源: `GET /keep/{resourceName}/{id}`
   - id: 資源 id
- 更新資料: `PUT /keep/{resourceName}/{id}`
   - body: 任意 json 物件
- 刪除資料: `DELETE /keep/{resourceName}/{id}`

mongodb 連線:

使用 [MongoDB Compass](https://www.mongodb.com/products/compass) 連線
host 位址為 `localhost:27017`

資料庫名稱為 `fake-data`


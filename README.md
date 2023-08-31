# fake-data

fake-data is an http api service that provides REST API for fake data return, temporary storage and NoSQL storage to facilitate testing during the development process of various applications that require http concatenation.


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

Use urlencoding to encode the expected return information and send it in query parameter: `feedback` to initiate a request

fake-data will be returned according to the set content.

feedback content format:

```json
{
    "contentType": "string", // Set the header: content-type attribute of response, the default is "application/json"
    "headers": {}, // set other response headers, the default is an empty object
    "status": 0, // set the response status code, the default is 200
    "body": {} // Set the response body content, the default is an empty object
}
```

example:

Expected response:

- content-type: text/plain
- status: 201
- headers: as follows
- body: as follows

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

Use urlencoding to encode the above json to get

```plain
%7B%0A%20%20%20%20%22contentType%22%3A%20%22text%2Fplain%22%2C%0A%20%20%20%20%22status%22%3A%20201%2C%0A %20%20%20%20%22headers%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22header-key%22%3A%20%22header-value %22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%22body%22%3A%20%7B%0A%20%20%20%20%20%20 %20%20%22data%22%20%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20%20%20%22string-key%22%3A %20%22some-string%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22int-key%22%3A%20100%2C%0A %20%20%20%20%20%20%20%20%20%20%20%20%22array-key%22%3A%20%5B%221%22%2C%202%2C%203.3%5D %0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D
```

Initiate a request:

```sh
curl -v localhost:8080/echo?feedback=%7B%0A%20%20%20%20%22contentType%22%3A%20%22text%2Fplain%22%2C%0A%20%20%20%20% 22status%22%3A%20201%2C%0A%20%20%20%20%22headers%22%3A%20%7B%0A%20%20%20%20%20%20%20%20%22header- key%22%3A%20%22header-value%22%0A%20%20%20%20%7D%2C%0A%20%20%20%20%22body%22%3A%20%7B%0A% 20%20%20%20%20%20%20%20%22data%22%20%3A%20%7B%0A%20%20%20%20%20%20%20%20%20%20% 20%20%22string-key%22%3A%20%22some-string%22%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22int- key%22%3A%20100%2C%0A%20%20%20%20%20%20%20%20%20%20%20%20%22array-key%22%3A%20%5B%221% 22%2C%202%2C%203.3%5D%0A%20%20%20%20%20%20%20%20%7D%0A%20%20%20%20%7D%0A%7D
```

echo API supports `GET`, `POST`, `PUT`, `DELETE` methods

## Cache API

path: `/cache/{resourceName}`

Simulate CRUD operations of REST API

Data will be stored in memory

Distinguish different resources according to `resourceName` in path


REST operations:

- New information: `POST /cache/{resourceName}`
    - body: any json object
- Get resource list: `GET /cache/{resourceName}`
- Get a single resource: `GET /cache/{resourceName}/{id}`
    - id: resource id
- Update data: `PUT /cache/{resourceName}/{id}`
    - body: any json object
- Delete data: `DELETE /cache/{resourceName}/{id}`

## Keep API

Simulate CRUD operations of REST API

The data will be stored in the mongodb database within the container

Mongodb opens port 27017 externally and can use other client-side software to connect.

REST operations:

- New information: `POST /keep/{resourceName}`
    - body: any json object
- Get resource list: `GET /keep/{resourceName}`
- Get a single resource: `GET /keep/{resourceName}/{id}`
    - id: resource id
- Update data: `PUT /keep/{resourceName}/{id}`
    - body: any json object
- Delete data: `DELETE /keep/{resourceName}/{id}`

mongodb connection:

Connect using [MongoDB Compass](https://www.mongodb.com/products/compass)
The host address is `localhost:27017`

The database name is `fake-data`

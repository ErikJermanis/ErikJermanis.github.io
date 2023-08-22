# Using cURL for API requests

## Motivation

When building APIs, you need a way to send requests to test out what you are building. For big projects, tools like [Postman](https://www.postman.com/) or [Insomnia](https://insomnia.rest/) are a great choice because they offer much more than just a way to send requests.

However, for small projects where all you need is a way to send requests to test your API, I find using [cURL](https://curl.se/) much more convenient.

## Sending requests

### GET and DELETE

If not specified differently, default HTTP method is GET.

```sh title="GET request"
curl <url>
```

```sh title="GET request example"
curl http://localhost:8000/api/v1/todos
```

To specify HTTP method, use `-X` option.

```sh title="DELETE request"
curl -X DELETE <url>
```

```sh title="DELETE request example"
curl -X DELETE http://localhost:8000/api/v1/todos/1
```

### POST, PUT and PATCH

For sending requests with data, use `-d` flag.

```sh title="POST request example"
curl -X POST -d @data.json http://localhost:8000/api/v1/todos
```

```sh title="PUT request example"
curl -X PUT -d @data.json http://localhost:8000/api/v1/todos
```

```sh title="PATCH request example"
curl -X PATCH -d @data.json http://localhost:8000/api/v1/todos
```

```json title="data.json" linenums="1"
{
	"title": "foo",
	"body": "bar",
	"userId": 1
}
```

If you are using `-d` flag, request is by default treated as POST request, so you can omit `-X` option.

```sh title="POST request example"
curl -d @data.json http://localhost:8000/api/v1/todos
```

### Other useful configuration

#### Query parameters

When using query parameters, wrap the url in `""` to avoid potential problems.

```sh title="GET request with query param"
curl "http://localhost:8000/api/v1/todos?id=1"
```

#### Sending headers

To send request headers use `-H` option.

```sh title="GET request with Authorization header"
curl -H "Authorization: Bearer <token>" http://localhost:8000/api/v1/todos
```

#### Reading response status and headers

To see information such as response status and response headers, use `-i` flag.

```sh
curl -i http://localhost:8000/api/v1/todos
```

## When things gets messy

When you need to send a request with multiple headers and a long url, it can quickly become a burden to type everything. You can easily end up with something like this:

```sh
curl -i -X POST -H "Content-Type: application/json" -H "Authorization: Bearer <token>" -d @data.json http://localhost:8000/api/v1/todos
```

To avoid that, I recommend using a cli tool like [endpoint](https://github.com/selectnull/endpoint).

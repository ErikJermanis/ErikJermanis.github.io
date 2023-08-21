# Using cURL for API requests

## Motivation

When building APIs, you need a way to send requests to test out what you are building. For big projects, tools like [Postman](https://www.postman.com/) or [Insomnia](https://insomnia.rest/) are a great choice because they offer much more than just a way to send requests.

However, for small projects where all you need is a way to send requests to test your API, I find using [cURL](https://curl.se/) much more convenient.

## Sending requests

Simple GET request:

```sh
curl <url>
```

Example:

```sh
curl http://localhost:8000/api/v1/
```

> TODO

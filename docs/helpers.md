## Helpers

### Interceptors

Axios plugin provides helpers to register axios interceptors easier and faster.

- `onRequest(config)`
- `onResponse(response)`
- `onError(err)`
- `onRequestError(err)`
- `onResponseError(err)`

This functions don't have to return anything by default.

Example: (`plugins/axios.js`)

```js
export default function ({ $axios, redirect }) {
  $axios.onError(error => {
    if(error.code === 500) {
      redirect('/sorry')
    }
  })
}
```

### Fetch Style requests

Axios plugin also supports fetch style requests with `$` prefixed methods:

```js
// Normal usage with axios
let data = (await $axios.get('...')).data

// Fetch Style
let data = await $axios.$get('...')
```

### `setHeader(name, value, scopes='common')`

Axios instance has a helper to easily set any header.

Parameters:

* **name**: Name of the header
* **value**: Value of the header
* **scopes**: Send only on specific type of requests. Defaults
  * Type: _Array_ or _String_
  * Defaults to `common` meaning all types of requests
  * Can be `get`, `post`, `delete`, ...

```js
// Adds header: `Authorization: 123` to all requests
this.$axios.setHeader('Authorization', '123')

// Overrides `Authorization` header with new value
this.$axios.setHeader('Authorization', '456')

// Adds header: `Content-Type: application/x-www-form-urlencoded` to only post requests
this.$axios.setHeader('Content-Type', 'application/x-www-form-urlencoded', [
  'post'
])

// Removes default Content-Type header from `post` scope
this.$axios.setHeader('Content-Type', false, ['post'])
```

### `setToken(token, type, scopes='common')`

Axios instance has an additional helper to easily set global authentication header.

Parameters:

* **token**: Authorization token
* **type**: Authorization token prefix(Usually `Bearer`).
* **scopes**: Send only on specific type of requests. Defaults
  * Type: _Array_ or _String_
  * Defaults to `common` meaning all types of requests
  * Can be `get`, `post`, `delete`, ...

```js
// Adds header: `Authorization: 123` to all requests
this.$axios.setToken('123')

// Overrides `Authorization` header with new value
this.$axios.setToken('456')

// Adds header: `Authorization: Bearer 123` to all requests
this.$axios.setToken('123', 'Bearer')

// Adds header: `Authorization: Bearer 123` to only post and delete requests
this.$axios.setToken('123', 'Bearer', ['post', 'delete'])

// Removes default Authorization header from `common` scope (all requests)
this.$axios.setToken(false)
```

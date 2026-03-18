# Database

All paths are relative — the app prepends the platform namespace internally.

---

## Calls

### `db_get`
Read a single value at a path.
```js
{ type: 'db_get', path: 'users/alice' }
// → { type: 'success', data: value | null }
// → { type: 'error', status: "Missing 'path' field." }
```

### `db_set`
Write a value at a path.
```js
{ type: 'db_set', path: 'users/alice', value: { name: 'Alice' } }
// → { type: 'success' }
// → { type: 'error', status: "Missing 'path' field." }
// → { type: 'error', status: "Missing 'value'." }
```

### `db_delete`
Remove a path and all its children.
```js
{ type: 'db_delete', path: 'users/alice' }
// → { type: 'success' }
// → { type: 'error', status: "Missing 'path' field." }
```

### `db_list`
List the keys at a path. Returns `[]` if the path doesn't exist.
```js
{ type: 'db_list', path: 'users' }
// → { type: 'success', data: ['alice', 'bob', ...] }
// → { type: 'error', status: "Missing 'path' field." }
```

### `db_get_all`
Read an entire subtree.
```js
{ type: 'db_get_all', path: 'users' }
// → { type: 'success', data: { alice: {...}, bob: {...} } | null }
// → { type: 'error', status: "Missing 'path' field." }
```

### `db_query`
Filter children at a path by matching fields. Returns `[]` if the path doesn't exist.
```js
{ type: 'db_query', path: 'users', filter: { role: 'admin' } }
// → { type: 'success', data: [...matching values] }
// → { type: 'error', status: "Missing 'path' field." }
```

### `db_subscribe`
Attach a live listener to a path. Fires `callback(path, value)` on any change. Replaces any existing listener for the same `subscriberId`.
```js
{ type: 'db_subscribe', path: 'users/alice', subscriberId: 'my-listener', callback: fn }
// → { type: 'success' }
// → { type: 'error', status: "Missing 'path' field." }
// → { type: 'error', status: "Missing 'subscriberId'." }
// → { type: 'error', status: "Missing 'callback' function." }
```

### `db_unsubscribe`
Remove a live listener. Silently succeeds if the `subscriberId` isn't active.
```js
{ type: 'db_unsubscribe', subscriberId: 'my-listener' }
// → { type: 'success' }
// → { type: 'error', status: "Missing 'subscriberId'." }
```

### `db_flush`
No-op — writes are real-time. Safe to call.
```js
{ type: 'db_flush' }
// → { type: 'success' }
```

### `db_stats`
Returns diagnostics about the connection and active subscribers.
```js
{ type: 'db_stats' }
// → { type: 'success', data: { topLevelKeys, backend, subscriberCount, subscribers } }
```
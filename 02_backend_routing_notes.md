# Backend Routing — Notes

## 1. What is Routing?

**Routing** is the process of mapping an incoming HTTP request to the appropriate server-side handler based on the URL path and HTTP method.

A route is generally identified by the combination of:

- **HTTP Method:** What action is requested (`GET`, `POST`, `PUT`, `PATCH`, `DELETE`).
- **URL Path:** Which resource the request targets.

Example:

```js
app.get('/users', (req, res) => {
    res.json(["Aman", "Rahul", "Priya"]);
});
```

A `GET` request to `/users` executes the associated handler and returns the list of users.

## 2. Static Routes

Static routes have a fixed URL path without variable parameters.

Examples:

```js
app.get('/api/books', handler);
app.post('/api/books', handler);
```

- `GET /api/books` — Fetch books.
- `POST /api/books` — Create a book.

The same path can handle different operations depending on the HTTP method.

## 3. Dynamic Routes and Path Parameters

Dynamic routes contain variable values within the URL path. They are useful when targeting specific resources.

Example:

```js
app.get('/api/users/:id', (req, res) => {
    const userId = req.params.id;
    res.send(`User ID: ${userId}`);
});
```

Request:

`GET /api/users/123`

Here, `123` is the path parameter, and `req.params.id` returns `"123"`.

**Key points:**
- Path parameters are accessed using `req.params` in Express.js.
- They identify a particular resource or a specific part of a resource collection.

## 4. Query Parameters

Query parameters pass additional information through the URL. They are commonly used for filtering, sorting, searching, and pagination.

They appear after `?` in the URL, with multiple parameters separated by `&`.

Example:

`GET /api/books?page=2&limit=10`

```js
app.get('/api/books', (req, res) => {
    const page = req.query.page;
    const limit = req.query.limit;

    res.json({ page, limit });
});
```

Here:
- `page=2` specifies the page number.
- `limit=10` specifies the number of books per page.
- `req.query` accesses query parameters in Express.js.

**Path parameters vs. query parameters**

| Path Parameters | Query Parameters |
|---|---|
| Identify a particular resource | Modify or refine a request |
| Part of the URL path | Appear after `?` |
| `/users/123` | `/users?page=2` |
| Accessed using `req.params` | Accessed using `req.query` |

## 5. Nested Routes

Nested routes represent relationships between resources through URL structure. They are common in REST APIs.

Example:

`GET /api/users/123/posts/456`

This can represent fetching post `456` belonging to user `123`.

```js
app.get('/api/users/:userId/posts/:postId', (req, res) => {
    const { userId, postId } = req.params;

    res.json({ userId, postId });
});
```

Here:
- `userId` identifies the user.
- `postId` identifies the post.
- The server should verify that the post actually belongs to that user.

Nested routes improve readability when the relationship between resources matters. They should not be nested excessively.

## 6. Route Versioning and Deprecation

**API versioning** allows an API to evolve without immediately breaking existing clients.

Example:

```text
/api/v1/products
/api/v2/products
```

- `v1` can continue supporting older clients.
- `v2` can introduce breaking changes, such as a different response structure.
- Older versions can eventually be deprecated and removed after an appropriate migration period.

Versioning is particularly useful for APIs used by multiple applications or frontend versions.

## 7. Catch-All Routes and 404 Handling

A catch-all or fallback route handles requests that do not match any defined route.

Example in Express.js:

```js
app.use((req, res) => {
    res.status(404).json({
        message: "Route not found"
    });
});
```

Place this middleware **after all valid routes** so that unmatched requests reach it.

A `404 Not Found` response indicates that the requested resource or route could not be found.

Note: Exact wildcard syntax varies between Express versions, so a final middleware is a straightforward option for handling unmatched requests.

## 8. HTTP Methods Used in Routing

These are important to understand alongside routing.

| Method | Purpose | Example |
|---|---|---|
| `GET` | Retrieve data | `GET /users` |
| `POST` | Create a resource | `POST /users` |
| `PUT` | Replace a resource | `PUT /users/123` |
| `PATCH` | Partially update a resource | `PATCH /users/123` |
| `DELETE` | Delete a resource | `DELETE /users/123` |

`PUT` and `PATCH` are both used for updates, but `PUT` generally replaces the resource representation, whereas `PATCH` applies partial modifications.

## 9. Route Handlers and Middleware

A **route handler** contains the logic that processes a request and sends a response.

**Middleware** is a function that can execute before the final handler to perform tasks such as authentication, logging, validation, or request processing.

Example:

```js
app.get('/profile', authenticateUser, (req, res) => {
    res.json({ message: "User profile" });
});
```

Here, `authenticateUser` is middleware that can verify the user before allowing the request to reach the handler.

In Express.js, middleware commonly uses the signature:

```js
(req, res, next) => {
    // Perform some operation
    next();
}
```

Calling `next()` passes control to the next middleware or handler, unless the response has already been sent or an error is forwarded.

## 10. Route Organization Using Express Router

As a backend grows, defining every route in one file becomes difficult to maintain. Express provides `Router` to group related routes into separate modules.

Example:

```js
// routes/userRoutes.js
const express = require('express');
const router = express.Router();

router.get('/', getUsers);
router.get('/:id', getUserById);
router.post('/', createUser);

module.exports = router;
```

Mount the router in the main application:

```js
const userRoutes = require('./routes/userRoutes');

app.use('/api/users', userRoutes);
```

The resulting endpoints include:

- `GET /api/users`
- `GET /api/users/:id`
- `POST /api/users`

This makes the application easier to maintain and scale.

## Quick Revision

- **Routing:** Maps HTTP methods and URL paths to server-side handlers.
- **Static route:** A fixed path, such as `/api/books`.
- **Dynamic route:** Contains variable path segments, such as `/api/users/:id`.
- **Path parameters:** Accessed through `req.params`.
- **Query parameters:** Accessed through `req.query`.
- **Nested route:** Represents a relationship between resources.
- **API versioning:** Maintains compatibility as an API evolves.
- **Catch-all route:** Handles unmatched requests, commonly with a `404`.
- **Middleware:** Performs operations before or around route handling.
- **Express Router:** Organizes routes into reusable modules.

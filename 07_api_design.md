## I. API Evolution & Web Architecture (0:00-21:50)

* *Historical Context**: In 1990, *Tim Berners-Lee pioneered the World Wide Web with fundamental components: URI*, *HTTP*, *HTML*, *Web Servers*, and *Web Browsers*. The *WYSIWYG editor was also a key innovation.
* **Transition to Modern Apps**: Shifted from multi-page applications to **Single Page Applications (SPAs)**, which are **client-heavy**. In SPAs, the browser downloads all necessary JavaScript in the first request and manages routing on the client side.
* *The REST Architectural Style**: Formalized by *Roy Fielding in 2000 to solve scalability issues for thousands of users.
    * **REST Name**: Stands for **Representational State Transfer**.
        * **Representational**: Data is shared via formats like *JSON*, *XML*, or *HTML*.
        * **State**: The current condition/attributes of a resource.
        * **Transfer**: Moving resource representations between client and server.
* **Core Constraints**: 
    * **Client-Server Model**: Decouples concerns.
    * **Uniform Interface**: A standardized communication method (includes resource identification and self-descriptive messages).
    * **Layered System**: Components see only the immediate layer below them (e.g., load balancers).
    * **Cache**: Responses must explicitly label their cacheability.
    * **Stateless**: Each request must contain all info needed for processing; the server stores no client context.
    * **Code on Demand**: Optional ability to transfer executable code (e.g., JavaScript) to clients.

## II. API Design & URL Best Practices (22:00-30:00)

* **URL Components**: `https://api.example.com/v1/resources/123?limit=10#header`
    * **Scheme**: Protocol (e.g., *https*).
    * **Domain**: Address (e.g., *api.example.com*).
    * **Versioning**: Always include versioning (e.g., */v1*) to manage changes.
    * *Resources**: Use **plural nouns* (e.g., /books instead of */book*). Use forward slashes to define hierarchy.
    * **Query Parameters**: Used for filtering, sorting, or pagination (e.g., *?limit=10*).
    * **Fragments**: (e.g., *#header*) help navigate within specific document parts.
* **Formatting**: Do not use spaces or underscores in URLs. Use hyphens for slugs (human-readable identifiers, e.g., *harry-potter*).

## III. HTTP Methods & Idempotency (31:00-46:20)

* **Idempotency**: An operation is idempotent if repeating the request produces the same result/state on the server.
    * **GET**: Always idempotent; used to fetch data.
    * *PUT/PATCH**: Idempotent; *PUT replaces the entire object, while PATCH updates specific fields.
    * **DELETE**: Idempotent; repeating it still removes the same resource.
    * **POST**: **Non-idempotent**; creates new objects or handles custom actions that don't fit CRUD paradigms.

## IV. API Development Workflow (47:00-1:04:40)

1. **Requirement Analysis**: Extract nouns (resources) from wireframes/Figma designs.
2. **Database Schema**: Define tables (e.g., Organization, Project, Task) based on identified resources.
3. *Interface Design**: Use tools like *Insomnia or Postman to design API routes before writing implementation code.
4. **Operations**: Identify needed CRUD (*Create, Read, Update, Delete*) actions for every resource.

## V. Pagination, Filtering & Sorting (1:05:20-1:20:00)

* *Pagination**: Prevents overloading the server/network. Use *limit and page as query parameters.
* *Response Metadata**: Always return *total count*, *current page*, and *total pages to help the client-side UI.
* **Filtering**: Allow clients to pass fields (e.g., *?status=active*) to refine returned lists.

## VI. Developer Experience & Best Practices (1:56:30-2:02:50)

* *Interactive Documentation**: Use tools like *Swagger or OpenAPI to provide a playground for testing.
* **Consistency**: Maintain uniform naming and structure across all API resources to eliminate client guesswork.
* **Sane Defaults**: Provide default values for parameters to make the API easier to use.
* **Readability**: Avoid cryptic abbreviations; keep field names intuitive.
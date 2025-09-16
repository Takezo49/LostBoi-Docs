# API - Endpoints

Good question 👌 — when you're testing or exploring a web application, it’s not always obvious whether you’re hitting a **normal web page** or an **API endpoint**. Here are some ways to figure it out:

***

#### 🔎 Signs You’re Interacting With an API Endpoint

1. **URL Structure**
   *   APIs often have URLs that look different from regular web pages.\
       Example patterns:

       ```
       http://site.com/api.php/city/london
       http://site.com/api/v1/users/123
       http://site.com/api/city?name=london
       ```
   * Notice things like:
     * `/api/` in the path
     * A table/row style (like `/city/london`)
     * Versioning (like `/v1/`, `/v2/`)
2. **Response Format**
   * API endpoints usually return **structured data**, not an HTML web page.\
     Common formats:
     * JSON → `{"city":"london","population":9000000}`
     * XML → `<city><name>london</name></city>`
   * If you see raw HTML with CSS/JS, that’s probably a normal page.
3. **Supported HTTP Methods**
   * A web page usually responds only to `GET` requests.
   * An API often supports:
     * `GET` (read data)
     * `POST` (create new data)
     * `PUT/PATCH` (update data)
     * `DELETE` (remove data)
   * You can test this with `curl` or tools like Burp/Postman.
4. **Headers**
   * APIs often expect or return special headers, like:
     * `Content-Type: application/json`
     * `Accept: application/json`
   * Web pages usually return `Content-Type: text/html`.
5. **Error Messages**
   *   API endpoints often return machine-readable errors:

       ```json
       {"error":"City not found"}
       ```
   * Instead of a big HTML error page.

***

#### 🛠 How to Check Practically

*   **Use `curl` or Postman**:

    ```bash
    curl -i http://<SERVER_IP>:<PORT>/api.php/city/london
    ```

    If the response is JSON/XML instead of HTML, you’ve got an API.
* **Check developer tools (Network tab)** in the browser:
  * When you search for "London," see what requests are sent.
  * If you see `/api/...` requests returning JSON, that’s the API.

***

👉 In short:\
You know you’re interacting with an **API endpoint** when the URL looks structured, the server responds with data (JSON/XML) instead of HTML, and multiple HTTP methods are supported.

# Server Side Template Injection

### Methodology

1. Identify all the Reflections of user controller input.
2. Check if our payload is being Evaluated through template engine.
3. Exploitation, Check for RCE.

### Understanding Basics

#### What is a template?

a **template** is a file that describes _how_ a page (or other text) should look, with **placeholders** where data will be inserted later. Think of it like a mail-merge letter: the letter is the template and the recipient names/addresses are the data plugged into placeholders.

Example :

![image.png](<../.gitbook/assets/image 7 (1).png>)

#### What is a template Engine?

A template engine combines a template (containing placeholders and logic) with data to produce a final document, like an HTML page.

Template engines are typically located and run on the server

Here's a basic example using JavaScript to illustrate the core concept:

**1. The Template:**

A simple string with placeholders indicated by double curly braces `{{ }}`:

```jsx
<p>Hello, my name is {{ name }} and I am {{ age }} years old.</p>
<p>I live in {{ city }}.</p>
```

**2. The Data:**

An object containing the values to replace the placeholders:

```jsx
const data = {
  name: "Alice",
  age: 30,
  city: "New York"
};
```

**3. The very Basic Template Engine Logic:**

A function that takes the template and data, and performs the replacement:

```jsx
function render(template, ctx) {
  // replace simple variables like {{name}} with string values
  return template.replace(/\\{\\{\\{?\\s*([\\w.]+)\\s*\\}?\\}\\}/g, (_, key) =>
    // very naive: raw if triple braces else escape
    RegExp.prototype.test.call(/^\\{\\{\\{/, arguments[0]) ? lookup(ctx, key) : escapeHtml(lookup(ctx, key))
  );
}

```

**4. Usage:**

```jsx
const template = `<p>Hello, my name is {{ name }} and I am {{ age }} years old.</p><p>I live in {{ city }}.</p>`;
const data = {
  name: "Alice",
  age: 30,
  city: "New York"
};

const renderedHtml = renderTemplate(template, data);
console.log(renderedHtml);
```

**Output:**

```jsx
<p>Hello, my name is Alice and I am 30 years old.</p><p>I live in New York.</p>
```

### In Depth Explanation

***

### 1. User submits input (browser → server)

The browser shows a form like:

```html
<form action="/event" method="POST">
  <input type="text" name="name" />
  <button type="submit">Submit</button>
</form>

```

👉 When the user types `Raja` and hits **Submit**, the browser sends a request to the **server**:

```
POST /event
Content-Type: application/x-www-form-urlencoded

name=Raja

```

***

### 2. Server receives the request

Your server route handles it. Example in Express:

```jsx
app.post("/event", (req, res) => {
  const userName = req.body.name;   // "Raja"
  // put the input into data object
  res.render("event", { event: { name: userName } });
});

```

Here’s the key:

* `res.render("event", data)` → loads the **event.ejs template** from disk.
* It runs the **renderer** (EJS in this case).
* The renderer replaces `<%= event.name %>` in the template with `"Raja"`.

***

### 3. Template gets rendered on the server

Suppose `event.ejs` looks like:

```
<h1>Event Details: <%= event.name %></h1>

```

The server uses the **renderer** and produces final HTML:

```html
<h1>Event Details: Raja</h1>

```

👉 At this point, the server has a **fully rendered HTML page** (a normal string).

***

### 4. Server sends the rendered HTML back

The server sends this rendered HTML as the HTTP response:

```
HTTP/1.1 200 OK
Content-Type: text/html

<h1>Event Details: Raja</h1>

```

***

### 5. Browser receives and displays the page

The browser gets that HTML response and replaces the current page with:

> Event Details: Raja

That’s why we say _the template is rendered on the server, then sent to the browser_.

***

#### 🔁 So the loop is:

1. User types input → sends request.
2. Server collects input → passes it as data to template engine.
3. Renderer (template engine) merges **template file + data** → produces final HTML.
4. Server sends that HTML in the HTTP response.
5. Browser displays it as a normal webpage.

***

***

### 1. What is `event.ejs`?

* It’s just a **text file** (like `.html`), but it contains **placeholders** for data.
* Example (`event.ejs` file):

```
<h1>Event Details: <%= event.name %></h1>
<p>Date: <%= event.date %></p>

```

Notice the `<%= ... %>` parts?

* These are _placeholders_.
* They tell the template engine: **“insert data here”**.

***

### 2. How does the server use it?

When the server runs this line:

```jsx
res.render("event", { event: { name: "TechConf", date: "2025-10-01" } });

```

* `"event"` → tells Express to look for `event.ejs` (usually in a `views/` folder).
* The `{ event: {...} }` part → is the **data object** we want to fill in.

***

### 3. What happens during rendering

* The template engine (EJS) opens `event.ejs`.
* It sees `<%= event.name %>` → replaces it with `"TechConf"`.
* It sees `<%= event.date %>` → replaces it with `"2025-10-01"`.

So after rendering, the final HTML string becomes:

```html
<h1>Event Details: TechConf</h1>
<p>Date: 2025-10-01</p>

```

***

### 4. How the browser gets it

That final HTML is what the server sends back to the browser as the **response**.

The browser doesn’t know about `<%= ... %>` — it only sees the finished HTML.

***

### 5. Visualizing the process

#### Template file (`event.ejs`)

```
<h1>Event Details: <%= event.name %></h1>
<p>Date: <%= event.date %></p>

```

#### Data object (from server)

```json
{
  "event": {
    "name": "TechConf",
    "date": "2025-10-01"
  }
}

```

#### Final result (HTML sent to browser)

```
<h1>Event Details: TechConf</h1>
<p>Date: 2025-10-01</p>

```

***

👉 So think of `event.ejs` like a **letter template**:

> "Dear <%= person %>, you are invited on <%= date %>."

If you pass `{ person: "Raja", date: "Monday" }`, it becomes:

> "Dear Raja, you are invited on Monday."

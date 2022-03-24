# Touring Vue Router
```
1. Receiving URL Parameters
2. Building Pagination
3. Nested Routes
5. Redirect & Alias
6. Programmatic Navigation
7. Error Handling and 404s
8. Flash Message
9. In-Component Route Guards
10. Global and Per-Route Guards
```
---

# Receiving URL Parameters

### Overview:

Overview of all different ways we can receive and parse URL data into our components with Vue Router. This will ensure we have the tools we need to build pagination.

### Problem: How do we read query parameters off the URL?

Often when we write pagination, we might have a URL that looks like this:
  > http://example.com/events?page=4

How can we get access to "page" inside our component?

### Solution: $route.query.page
Inside our component to read the page listing all we need to do inside our template is write:
```JavaScript
<h1>You are on page {{ $route.query.page }}</h1>
```

To get access from inside component code, we'll need to add "this.":
```JavaScript
computed: {
  page() {
    return parseInt(this.$route.query.page) || 1
  },
}
```

### Problem: What if we wanted the page to be part of the URL?

There are some cases in web development wher you might want the page number to be an actual part of the url, instead of in the query parameters (which come after a question mark).

### Solution: Route Parameter
To solve this we'd likely have a route that looked like:

```JavaScript
const routes = [
  ...
  { path: '/events/:page', component: Events},
]
```

Then inside our event component, we could access this in the template as such:
```JavaScript
<h1>You are on page {{ $route.params.page }}</h1>
```

Notice that in this case we are using "$route.params" instead of "$route.query" as we did above.

### Bonus: Passing Params as Props

If we want to make our component more reusable and testable, we can decouple it from the route by telling our router to pass our Param "page" as a Component prop. To do this, inside our router we would write:
```JavaScript
const routes = [
  ...
  { path: '/events/:page', component: Events, props: true },
]
```

Then inside our component we would have:
```JavaScript
<template>
  <h1>You are on page {{ page }}</h1>
</template>
<script>
export default {
  props: ["page"]
};
</script>
```


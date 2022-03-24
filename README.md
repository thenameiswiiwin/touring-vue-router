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

# 1. Receiving URL Parameters

## Overview:

Overview of all different ways we can receive and parse URL data into our components with Vue Router. This will ensure we have the tools we need to build pagination.

## Problem: How do we read query parameters off the URL?

Often when we write pagination, we might have a URL that looks like this:

> http://example.com/events?page=4

How can we get access to "page" inside our component?

## Solution: $route.query.page

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

## Problem: What if we wanted the page to be part of the URL?

There are some cases in web development wher you might want the page number to be an actual part of the url, instead of in the query parameters (which come after a question mark).

## Solution: Route Parameter

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

## Bonus: Passing Params as Props

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

Noticed we are delcaring "page" as a prop and rendering it to the page.

## Problem: Route level configuration

Sometimes we have a component we want to be able to configure at the route level. For example, if there is extra information we want to display or not.

## Solution: Props Object Mode

When we want to send configuration into a component from the router, it might look like this:

```JavaScript
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
    props: { showExtra: true },
  },
]
```

Noticed the static props object with "showExtra". We can the receive this as a porp in our component, and do something with it:

```JavaScript
<template>
  <div class="home">
    <h1>This is a home page</h1>
    <div v-if="showExtra">Extra stuff</div>
  </div>
</template>
<script>
export default {
  props: ["showExtra"]
};
</script>
```

## Problem: How to transform query parameters?

Sometimes you may have a situation where the data getting sent into your query parameters needs to be transformed before it reaches your component. For example, you may want to cast parameters into other types, rename them, or combine values.

In our case let’s assume our URL is sending in "e=true" as a query parameter, while our component wants to receive "showExtra=true" using the same component in the above example. How could we do this transform?

## Solution: Props Function Mode

In our route to solve this problem we would write:

```JavaScript
const routes = [
  {
    path: "/",
    name: "Home",
    component: Home,
    props: (route) => ({ showExtra: route.query.e }),
  },
```

Notice we’re sending in an anonymous function which receives the "route" as an argument, then pulls out the query parameter called "e" and maps that to the "showExtra" prop.

The anonymous function above could also be written like:
```JavaScript
props: route => {
  return { showExtra: route.query.e }
}
```

It’s a little more verbose, but I wanted to show you this to remind you that you could place complex transformations or validations inside this function.

---

# 2. Building Pagination

## Overview:

Pagination is a very common piece of functionality, found in many applications.



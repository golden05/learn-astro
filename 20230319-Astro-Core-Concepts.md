---
tags = [Astro]
version = 2.1
---

# why Astro

## Content-focused: content-rich

includes most marketing sites, publishing sites, document sites, blog, portflios

## Server-first: render HTML on the server

multi Page App MPA

## Fast by default:

load 40% faster with 90% less javascript

## Easy to use:

use any favorite UI component .astro UI language

## fully-featured, flexible: Over 100+ astro integrations

component syntax, file-based routing, asset handling, a build process, bundling, optimizations, data-fetching

# MPA vs SPA

## MPA vs MPA

other language, astro use javascript ，HTML and CSS

## MPA vs SPA: three main difference

- Server rendering vs Client rendering
- server routing vs client routing
- MPA server state management vs client state management

# Astro Islands component Islands

## what is an astro island

multiple Islands can exist on a page. Islands in a sea of static, non-interactive HTML
in Astro can use any supported UI framework like react, Svelte or Vue to render island in browser.
partial or selective hydration.

## how do islands work in Astro

Use a frontend UI component built with React, Svelte, Vue, SolidJS and Astro will automatically render it to HTML.
then strip out all of the javascript

```javascript
import MyReactComponent from "../components/MyReactComponent.jsx";
<MyReactComponent />;
```

forcing your entire page to become an SPA-like javascript application.

```
import MyReactComponent from "../components/MyReactComponent.jsx";
<MyReactComponent client:load />
```

## what are the benefits of islands

- performance , converted to fast. static HTML, and javascript is only loader for individual components
  javascript is one of the slowest assets
- parallel loading

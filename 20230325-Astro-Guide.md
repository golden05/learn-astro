---
title: Astro Guide
tags: [Astro]
version: 2.1
---

# Astro Syntax

## JSX-like expression

inside two --- , frontmatter . can inject variables

1. variables
2. Dynamic Attributes : local variables use curly braces in the body or \${}
   HTML attributes will be converted to strings, don't assign an event handler
   `<button onClick=--{handleClick}--</button>`
   use a client-side script to add event handlers
3. Dynamic HTML : local variables use curly braces in the body to generate HTML element
4. Dynamic Tags : set variables to an HTML tag name or a component import, tag name must capitialized

```
---
const Element = 'div'
---
<Element> Hello </Element>
```

5. Fragments & Multiple Fragments: <>
6. Different between Astro and JSX :

- Attributes: use standard `kebab-case` instead of `camelCase`

# UI Frameworks

## Install Integrations

## Using Frameworks components

all your components in `src/components`

## Hydrating Interactive components

using a `client:*` directive to active. These are component attributes that determine when your component's JavaScript
should be sent to the browser.
with all client directive except `client:only`, your component will first render on the server
to generate static HTML. The component will then hydrate and become interactive.
js Frameworks(React,Svelte) need to render the component will be sent to the browser
alone with the component's own JavaScript.

### Hydrating Directives

`client:load`,`client:idle`, `client:visible`, `client:media={QUERY}`, `client:only={FRAMEWORK}`

## Mixing Frameworks

only Astro component `.astro` can contain components from multiple frameworks

## Passing Props to Framework components

pass a function as a prop to a framework component

## Passing Children to Framework components

inside of an astro component, can pass children to framework components.

- React, Preact and Solid use prop named `children`
- Svelte and Vue use the `<slot />`element
- all use Named Slots  
  `named-slots.astro`: `<h2 slot='title'>Menu</h2>`
  `Mysidebar.jsx`: `<header>{props.title}</header>`

## Nesting Frameworks components

can recursively nest framework components from any of these frameworks

## use Astro Components inside my Framework Components

cannot import `.astro`components in a UI framework component
can use Astro `<slot />` pattern pass staticcontent as children to your framework
inside a `.astro` **component**.
`src/pages/astro-children.astro` :

```
---
import MyReactComponent from '../components/MyReactComponent.jsx';
import MyAstroComponent from '../components/MyAstroComponent.astro';
---
<MyReactComponent>
  <MyAstroComponent slot='name' />
</MyReactComponent>
```

# Routing

Astro use **file-based routing** to generate your build URLs base file layout.

## Navigating between pages: use `<a>` elements to navigate between routes

```
<p>Read more <a href="/about/">about</a> Astro</p>
```

## static routes

in the `src/pages/` directory **automatically become pages on your website**
Each page's route corresponds to its path .

```
src/pages/index.astro  --> mysite.com/
src/pages/about.astro --> mysite.com/about
src/pages/about/index.astro --> mysite.com/about
src/pages/about/me.astro --> mysite.com/about/me
src/pages/posts/1.md --> mysite.com/posts/1
```

## Dynamic routes

An astro page file can specify dynamic routes parameters in its filename to generate
For example `src/pages/authors/[author].astro` generate a bio page for every author .
`author` become a parameter

### Static (SSG) Mode

a dynamic route must export a `getStaticPaths()` that returns an array of objects with `params` property.
`[dog].astro` defines the dynamic `dog` in their `params` .
The page can access this parameter using `Astro.params`.
The filename can include multiple parameters, which must all be include in the
`params` objects in `getStaticPaths()`.
`src/pages/[lang]-[version]/info.astro`:

```
---
export function getStaticPaths(){
  return [
    {params: {lang: 'en', version: "v1" }},
    {params: {lang: 'fr', version: "v2" }},
    ]
  }
const { lang, version } = Astro.params;
---
```

# Markdown & MDX

# Content Collections

# Script & Event handling

# CSS & Styling

# Assets

# Images

# Fonts

# Imports

# Server-side Rendering

# Endpoints

# Data Fetching

# Testing

# Troubleshooting

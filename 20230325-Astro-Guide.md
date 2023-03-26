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

all route according to getStaticPaths() return an array of params's object
option return an array of props object
destruct can {} from Astro.params  
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
For example 'src/pages/authors/[author].astro' generate
'author' become a parameter

- in SSR mode, a page will be generated on request for any route .

### Static (SSG) Mode

动态创建的页面和路由会在构建时生成
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

#### Rest parameters

more flexibility in your URL routing . use a **rest parameter**
`([...path])` in your `.astro`.
route 根据文件路径任一层级的多个参数生成
`src/pages/sequences[...path].astro`

```
---
export function getStaticPaths(){
    return [
     { params: {path: 'one/two/three' }},
     { params: {path: 'four' }},
     { params: {path:  undefined }}
     ]
  }
const { path } = Astro.params;
---
```

will generate /sequences/one/two/three, sequences/four,
and /sequences.(undefined allows it to match top level page)

Rest parameters be used with **named parameters**.
`/[org]/[repo]/tree/[branch]/[...file]`
/withastro/astro/tree/main/docs/public/favicon.svg

```
{
    org: 'withastro',
    repo: 'astro',
    branch: 'main',
    file: 'docs/public/favicon.svg'
  }
```

#### Dynamic pages at multiple levels

in the following example a rest parameter ([...slug]) and **props**
`getStaticPaths()` generate pages for slugs of different depths.

`src/pages[...slug].astro`

```
---
export async function getStaticPaths(){
    const pages = [
    {
        slug: undefined,
        title: "Astro Store",
        text: "Welcome to the Astro Store!",
    },
    {
        slug: "products",
        title: "Astro products",
        text: "We have lots of products for you",
    },
    {
        slug: "products/astro-handbook",
        title: "The ultimate Astro handbook",
        text: "If you want to learn Astro "
    },
    ];
    return pages.map(({slug, title,text}) => {
        return {
            params: { slug },
            props: { title, text },
          };
      });
    }

const { title, text} = Astro.props;
---
<html>
  <head>
    <title>{title}</title>}
  </head>
  <body>
    <h1>{title}</h1>
    <p>{text}</p>
  </body>
</html>
```

排序

- 没有路径参数的静态路由优先
- 使用命名参数的动态路由优先其余参数 rest
- 其他参数的优先级 rest 最低
- 平局按字母顺序解决
- `pages/posts/create.astro` => `/posts/create`
- `pages/posts/[pid].astro` => `/posts/1`, `/posts/abc`
- `pages/posts/[...slug].astro` => `/posts/1/2`, `posts/a/b/c`

### Server (SSR) Mode Server-side Rendering

dynamic routes are defined the same way : include `[params]` or `[...path]`
Since these are not "static" routes, `getStaticPaths` should not be used .

```
---
const { resource, id } = Astro.params;
---
<h1>{resource}: {id}</h1>}
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

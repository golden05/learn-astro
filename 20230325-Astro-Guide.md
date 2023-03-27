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

### Server (SSR) Mode Server-side Rendering

dynamic routes are defined the same way : include `[params]` or `[...path]`
Since these are not "static" routes, `getStaticPaths` should not be used .
`src/pages/resources/[resource]/[id].astro`

```
---
const { resource, id } = Astro.params;
---
<h1>{resource}: {id}</h1>}
```

`resource` and `id`: `resources/users/1`, `resources/colors/blue`

#### modifying the [...slug] example for SSR

SSR page can't use `getStaticPaths()`, can't receive props.
`const { slug } = Astro.params;`
`if (!page) return Astro.redirect("/404");`

## Route Priority Order

- Static routes without path parameters will take precedence
- Dynamic routes using named parameters
- Rest parameters have the lowest Priority
- alphabetically

## Pagination

built-in pagination . will generate common pagination properties, including previous/next page URL
total number of pages.
Pagination route name use the same [bracket] syntax as a standard dynamic route.
`/astronauts/[page].astro` will generate router for `/astronauts/1`.
where `[page]` is the page number.
`paginate()` funcwill to generate these pages
`src/pages/astronauts/[page].astro`

```
---
export async function getStaticPaths( { paginate}) {
    const astronautPage = [{
        astronaut: 'Noel Aremstrong',
      },{
        astronaut: 'Buzz Aldrin',
      }];
      return paginate(astronautPage), { pageSize: 2 });
  }
const { page } = Astro.props;
---
<h1>Page {page.currentPage}</h1>
<ul>
  {page.data.map(({ astronaut }) => <li>{astronaut}</li>)}
</ul>
```

### the `page` prop

use `paginate()` function , each page will be passed its data via a `page` prop.
The `page` prop has many useful properties.

- page.data : array containing the page's slice of data that passed to `paginate()`
- page.url.next : next page
- page.url.prev : previous page

```
{page.url.prev ? <a href={page.url.prev}>previous</a> : null}
{page.url.next ? <a href={page.url.next}>next</a> : null}
```

### Complete API reference

```
interface Page<T = any> {
  data: T[];
  start: number; end: number; total: number; currentPage: number;
  size: number; lastPage: number;
  url {
      current: string;
      prev: string | undefined; next: string | undefined;
    }
}
```

### Excluding pages

prefix their name with an underscore `_`

# Markdown & MDX

built-in, mdx Markdown support JSX, with [@astro/mdx](https://docs.astro.build/en/guides/integration-guide/mdx/)

## Markdown and MDX Pages

### Content Collections

manage Markdown and MDX in `src/content` folder
[Content Collections](https://astro.build/content-collections) organize content,validate frontmatter

### file-based Routing

Astro treats any `.md` or `.mdx` file inside of the `src/pages` directory as a page
example: `src/pages/page-1.md`.

### draft Pages

`draft: true` is an optional frontmatter value as unpublished

#### enable building draft

update `astro.config.mjs` by adding `draft: true` to `markdown`

## Markdown Features

### Frontmatter `layout`

`layout` property to specify : is layout component.
throuth `Astro.props` can access frontmatter properties:

```
---
const {frontmatter} = Astro.props;
---
<h1>{frontmatter.title}</h1>
```

1. access frontmatter by `content` properties
2. `<slot />` define content position

`frontmatter` has an `astro` property, can include Markdown's `source` and `headers`

```
"astro" : {
    "headers": [
    {
        "depths": 1,
        "text": "Astro 0.18 Release",
        "slug": "astro-018-release"
    },
    {
        ...
      }
    ],
    "source": "# Astro 0.18"
  },
  "url": ""
}
```

### Heading IDs

all head in md automatically give you anchor links so you can link directly to certain sections of your page

### Escaping special

need to use a different syntax if you want to display them.
`<` write `&lt;` , `{` write `&lcub;`

## MDX-only Features : Adding Astro [MDX Integration](https://docs.astro.build/en/guide/integrations-guide/mdx/)

`.mdx` file written in [MDX syntax](https://mdxjs.com/docs/what-is-mdx/#mdx-syntax)

### Using Exported Variables in MDX

support `export` statements to add variables to your MDX content.
these variables can accessible in the template itself and as named properties
`src/pages/posts/posts-1.mdx`: can export a `title` field or component to use as a heading with
`{JSX expressions}`:

```
export const title = 'My first MDX post'
# {title}
```

### Using Frontmatter Variables in MDX

support using frontmatter in MDX by default. frontmatter properties just as you would in MD files.
these variables are accessible to use in the template , in its layout component.
`src/pages/posts/post-1.mdx`:

```
---
layout: '../../layouts/BlogPostLayout.astro'
title: 'My first MDX support'
---
# {frontmatter.title}
```

### Using Components in MDX

can import and use both Astro Components and UI Framework components in MDX .
do't forget include a `client:directive` on your UI framework component, if necessary!
`src/pages/about.mdx`

```
---
layout: '../layouts/BaseLayout.astro'
title: About me
---
import Button from '../components/Button.astro';
import ReactCounter from '../components/ReactCounter.jsx';
I live on **Mars** but feel free to <Button title="Contact me" />
<ReactCounter client:load />
```

### Assigning Custom Components to HTML elements

`src/pages/about.mdx`

```
import Blockquote from '../components/Blockquote.astro';
export const components = { blockquote: Blockquote }
```

## import MDX

can import MD and MDX file directly into your Astro files.
can use a `import`, or multiple pages with `Astro.glob()`

```
import * as myPost from '../pages/post/my-post.md';
const posts = await Astro.glob('../pages/post/*.md');
```

in MDX files can access properties from both frontmatter and `export` statements.

### Exported properties

the following properties are available in an `.astro` component when using an `import` or `glob()`

- file
- url
- frontmatter
- getHeadings
- Content

### the `Content` component

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

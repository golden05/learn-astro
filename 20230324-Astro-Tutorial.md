---
title: Astro Tutorial
tags: [Astro]
version: 2.1
---

# Add, style and link to page on your site

## Build first Astro Blog

- setup your development environment
- create pages and blog posts for your website.
- build with Astro components
- Query and work with local files.
- Add interactivity to your site.
- deploy your site to the web

### Launch the Astro setup wizard

```
npm create astro@latest
npm run dev
```

### edit your home page : `index.astro`

```astro
<html lang="en">
  <head>
    <meta charset="utf-8" />
    <link rel="icon" type="image/svg+xml" href="/favicon.ico" />
    <meta name="viewport" content="width=device-width" />
    <meta name="generator" content={Astro.generator} >
    <title>Astro</title>
  </head>
  <body>
    <h1>My Astro Site</h1>
  </body>
</html>
```

### Create a new `.astro` file

under `src/pages/` create a new file named `about.astro`.

```
<body>
  <h1>About Me</h1>
  <h2>   and my new AStro site!</h2>

  <p>I am working through Astro's introductory tutrial.
</body>
```

### Add navigation links

```
<a href="/">Home</a>
<a href="/about">About</a>
```

## write first markdown blog post

create a new directory at `src/pages/posts/`, add a new file post-1.md
browser adding `posts/post-1`

### content:

```
---
title: 'My First Blog Post'
pubDate: 2022-07-01
description: 'This is the first post f my new Astro blog!'
author: 'Astro leader'
image:
    url: 'https://astro.build/assets/blog/astro-1-release-update/cover.jpeg'
    alt: 'The Astro logo with the word One.'
tags: ["astro", "blogging", "learn in publicity"]
---
# My first blog post
```

### Link to your blog posts

```
<ul>
  <li><a href='/posts/post-1'>Post 1</a></li>
</ul>
```

## Add dynamic content about you

### Define and use a variable

add Javascript in the `about.astro` in the frontmatter script
and use it in body section

```
---
const pageTitle = "About Me";
---
<title>{pageTitle}</title>

```

### write Javascript expressions in Astro

add the following JavaScript expressions to your frontmatter.

```
---
const identity = {
    firstName: 'Sarah',
    country: 'Canada',
    occupation: 'Technical Writer',
    hobbies: ["photograph","birdwatching"],
  };
const skills = ["html", "css", "javascript", "React", "Astro"];
---
<li> My name is {identity.firstName} </li>
<li>Two of my hobbies are {identity.hobbies[0]}
<ul>
  {skills.map((skill) => <li>{skill}</li>)}
</ul>
```

## conditionall render elements

add the following JavaScript expressions to your frontmatter

```
---
const happy = true;
const goal = 3;
---
{happy && <p>I am happy to learning Astro</p>}
{ goal === 3 ? <p>My goal is 3</p> : <p>My goal isn't 3</p> }
```

## Style your About page

### Style an individual page

use Astro own <style></style>, adding attributes and directives to these tags

```
<head>
  <style>
    h1 {
        color: purple;
        font-size: 4rem;
      }
    .skill {
        color: green;
        font-weight: bold;
      }
  </style>
</head>

<ul>
  {skills.map(skill) => <li class="skill">{skill}</li>)}
</ul>
```

### Use your first CSS variable

an style tag can reference any variable from your frontmatter script

```
---
const skillColor = "navy";
---
<style define:vars={{skillColor}}>
  .skill {
      color: var(--skillColor);
    }
</style>
```

## Add site-wide styling

### Add a global stylesheet

create a new file at the location `src/styles/global.css` , global.css like this:

```
html {
background-color: red;
font-family: sans-serif;
}
```

in the `about.astro` , import to the frontmatter

```
---
## import '../styles/global.css';
---
```

# Components

reuse code for common elements

- A Navigation component that present menu
- A Footer component that
- A Social Media component, used in the Footer or right-top
- An interactive Hamburger component to toggle the Navigation or mobile

## make a reusable Navigation component

- create a new folder `src/components` for components
- build an Astro component
- Replace existing HTML with a new Navigation component

a new component file `Navigation.astro` like this:

```
<a href="/">Home</a>
<a href="/about">About</a>
```

### import and use Navigation.astro
`src/pages/index.astro`
```
---
import Navigation from `../components/Navigation.astro`
---
<Navigation />
```

## Create a social media footer

### Create a Footer Component

Create a new file in the location `src/components/Footer.astro`

```
---
const platform = "github";
const username = "withAstro";
---
<footer>
  <p>Learn more about <a href={`https://www.${platform}.com/${username}`}></a></p>
</footer>
```

### import and use `Footer.astro`

```
import Footer from '../components/Footer.astro';
<Footer />
```

### Create a Social Media component

each time you will pass it different properties { props }
create a new file in the location `src/components/Social.astro`

```
---
const {platform, username} = Astro.props;
---
<a href='https://www.${platform}.com/${username}'}>{platform}</a>
```

### import and use `Social.astro`

```
---
import Social from './Social.astro';
---
<footer>
  <Social platform="youtube" username="astrobuild"/>
</footer>
```

## Build it yourself - Header

### Build a new Header component

create a file named `Header.astro` in `src/components/`

```
---
import Navigation from './Navigation.astro'
---
<header>
  <nav>
    <Navigation />
  </nav>
</header>
```

### Update your page: `src/pages/index.astro`

```
---
import Header from '../components/Header.astro';
---
<Header />
```

## Send your first script to the browser

add a hamburger menu to open and close your links on mobile screen sizes. requiring some clientside interactivity.

### build a Hamburger component

create a new file named `Hamburger.astro` in `src/components`

```
<div class='hamburger'>
  <span class='line'></span>
  <span class='line'></span>
  <span class='line'></span>
</div>
```

### write your first script tag

in the file `src/pages/index.astro`

```
<script>
  document.querySelector('hamburger').addEventListener('click',() => {
      document.querySelector('.nav-links').classList.toggle('expanded');
    });
</script>
```

# Layouts

## Build your first layout

### Create your first layout component

create a new file in the location `src/layouts/BaseLayout.astro`
and in your home page use `import BaseLayout from "";`

### Pass page-specific values as props: **src/pages/index.astro**

```
---
import BaseLayout from '../layouts/BaseLayout.astro';
const pageTitle = 'Home Page';
or
const pageTitle = Astro.props;
---
<BaseLayout pageTitle={pageTitle}>
```

## create and pass data to a custom blog layout

### add a layout to your blog post

create a new file in the location `src/layouts/MarkdownPostLayout.astro`

```
---
const {frontmatter} = Astro.props;
---
<h1>{frontmatter.title}</h1>
<p>Written by {frontmatter.author}</p>
```

Add to the post-1.md

```
---
layout: ../../layouts/MarkdownPostLayout.astro
---
```

## Combine layouts to get the best

### Nest your two layouts

two layout file : `BaseLayout.astro` and `MarkdownPostLayout.astro`
in the MarkdownPostLayout.astro

```
---
import BaseLayout from './BaseLayout.astro';
const { frontmatter } = Astro.props;
---
<BaseLayout pageTitle = {frontmatter.title}>
```

# Astro API

- `Astro.glob()` to access data from files in your project
- `getStaticPaths()` to create multiple pages
- Astro RSS package to create RSS feed

## create a blog post archive

### dynamiclly display list of posts

add following code to `blog.astro` , Astro.glob() will return an array of objects

```
---
const allPosts = await Astro.glob('../pages/posts/*.md');
---
{allPosts.map((post) => <li><a href={post.url}>{post.frontmatter.title}}</a></li>)}
```

## Generate tag pages

### dynamic page routing

you can create entire sets of page dynamiclly using `.astro` files that export a `getStaticPaths()`

### create pages dynamiclly

1. create a new file at the `src/pages/tags/[tag].astro`, file like this:

```
---
export async function getStaticPaths(){
    return [
      { params: {tag: "astro"} },
      { params: {tag: "successes"} },
      { params: {tag: "community"} },
    ];
  }
const { tag } = Astro.params;
---
<BaseLayout pageTitle={tag}>

```

### Use props in dynamic routes

1. Add the following props to getStaticPaths() to make data from all your blog posts
   `src/pages/tags/[tag].astro`

```
---
import BaseLayout from '../layout/BaseLayout.astro';
export async function getStaticPaths(){
  const allPosts = await Astro.glob('../posts/*.md');

  return [
    { params: {tag: "astro"}, props: {posts: allPosts}},
    { params: {tag: "successes"}, props: {posts: allPosts}},
    { params: {tag: "community"}, props: {posts: allPosts}}
    ]
}
const { tag } = Astro.params;
const { posts } = Astro.props;
---
```

2. Filter your list of posts to only include that contain the page's own tag
   `src/pages/tags/[tag].astro`

```
---
const { tag } = Astro.params;
const { posts } = Astro.props;
const filteredPosts = posts.filter((post) => post.frontmatter.tags.includes(tag));
---
```

3. you update your HTML template to show a list of each blog post containing the page's own tag
   `src/pages/tags/[tag].astro`

```
<BaseLayout pageTitle={tag}>
  <p>Posts tagged with {tag}</p>
  <ul>
    {filteredPosts.map((post) => <li><a href={post.url}>{post.frontmatter.title}</a></li>)}
  </ul>
</BaseLayout>
```

4. refactor this to use your `<BlogPost />` component
   `src/pages/tags/[tag].astro`

```
<BaseLayout pageTitle={tag}>
  <p>Posts tagged with {tag}</p>
  <ul>
    {filteredPosts.map((post) => <BlogPost url={post.url} title={post.frontmatter.title}/>}
  </ul>
</BaseLayout>
```

### Advanced Javascript: Generate pages from existing tags

your tag pages are now defined in `[tag].astro`. if add a new tag,
have to revisit this page and update your page routes.

1. Check that all your blog posts contain tags in its frontmatter  
   revisit each of your existing posts

2. Create an array of all your existing tags.
   `src/pages/tags/[tag].astro`

```
---
import BaseLayout from '../layout/BaseLayout.astro';
export async function getStaticPaths(){
  const allPosts = await Astro.glob('../posts/*.md');
  const uniqueTags = [...new Set(allPosts.map((post) => post.frontmatter.tags).flat())];
  // leavage map get every post's frontmatter tags and flatten it
  // leavage Set unique
  return uniqueTags.map((tag) => {
      const filteredPosts = allPosts.filter((post) => post.frontmatter.tags.includes(tag));
      return {
        params: {tag},
        props: {posts: filteredPosts},
        };
    });
  }
---
```

note: `getStaticPaths` function return a list of objects containing:

- `params` : what to call each page route
- optional `props` :data that you want to pass to those pages

```
---
const { tag } = Astro.params;
const { posts } = Astro.props;
---
<ul>
  {posts.map((post) => <BlogPost url = {post.url} title = {post.frontmatter.title}/>)}
</ul>
```

## Build a tag index page

### Use the `/folder/index.astro` routing pattern

create a new file at `src/pages/tags/index.astro`

```
---
import BaseLayout from '../../layouts/BaseLayout.astro';
const tags = ["astro", "successes", "community"];
const allPosts = await Astro.glob('../posts/*.md');
const tags = [...new Set(allPosts.map((post) => post.frontmatter.tags).flat())];
const pageTitle = 'Tag Index';
---
<BaseLayout pageTitle={pageTitle}>
  <div>
    {tags.map((tag) => (
      <p><a href={`/tags/${tag}`}>{tag}</a></p>
      ))}
  </div>
</BaseLayout>
```

### Add styles to your tag list

#### Add this page to your navigation

`src/components/Navigation.astro` : `<a href="/tags/>Tags</a>`

## Add an RSS feed

### Install Astro RSS package

`npm install @astrojs/rss`

#### Create an `.xml` feed document

1. Create an new file in `src/pages` called `rss.xml.js`
2. code :

```
import rss, { pagesGlobToRssItems } from '@astrojs/rss';
export async function get(){
    return rss({
        title: 'Astro Learner',
        description: 'Astro Learning Blog',
        site: 'https://astrojs.orr',
        items: await pagesGlobToRssItems(import.meta.glob('./**/*.md')),
        customData: `<language>en-us</language>`,
      });
  }


npm run build
npm run preview
```

# Astro Islands

## Build your first Astro Island

### Add Preact to Astro : `npx astro add preact`

### include a Preact greeting banner

- Create new file in `src/components/Greeting.jsx`

```
import { h } from 'preact';
import { useState } from 'preact/hooks';

export default function Greeting({messages}){
    const randomMessage = () => messages[Math.floor(Math.random() * messages.length)];
    const [greeting, setGreeting] = useState(messages[0]);
    return (
        <div>
            <h1>{greeting}! Thank you for visiting! </h1>
            <button onClick={() => setGreeting(randomMessage())}>
            New Greeting
            </button>
        </div>
    );
  }
```

- import and use this component in `index.astro`
  note `client:load` directive to tells Astro to send and rerun its Javascript to the _client_\
  when the page loads, making the component interactive.
  call this a **hydrated** component

```
---
import Greeting from '../components/Greeting.jsx';
---
<Greeting client:load messages={["h1", "Hello"]}/>
```

they are other `client:` directive to explore. Each sends the javascript to the client at the different time.
`client:visible`: only send the component's javascript when it is visible on the page.

## day to night

### Add and style a theme toggle icon

`src/components/ThemeIcon.astro`

```
<button id="themeToggle">
  <svg width="30px" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 24 24">
    <path class="sun" fill-rule="evenodd" d="M12 17.5a5.5 5.5 0 1 0 0-11 5.5 5.5 0 0 0 0 11zm0 1.5a7 7 0 1 0 0-14 7 7 0 0 0 0 14zm12-7a.8.8 0 0 1-.8.8h-2.4a.8.8 0 0 1 0-1.6h2.4a.8.8 0 0 1 .8.8zM4 12a.8.8 0 0 1-.8.8H.8a.8.8 0 0 1 0-1.6h2.5a.8.8 0 0 1 .8.8zm16.5-8.5a.8.8 0 0 1 0 1l-1.8 1.8a.8.8 0 0 1-1-1l1.7-1.8a.8.8 0 0 1 1 0zM6.3 17.7a.8.8 0 0 1 0 1l-1.7 1.8a.8.8 0 1 1-1-1l1.7-1.8a.8.8 0 0 1 1 0zM12 0a.8.8 0 0 1 .8.8v2.5a.8.8 0 0 1-1.6 0V.8A.8.8 0 0 1 12 0zm0 20a.8.8 0 0 1 .8.8v2.4a.8.8 0 0 1-1.6 0v-2.4a.8.8 0 0 1 .8-.8zM3.5 3.5a.8.8 0 0 1 1 0l1.8 1.8a.8.8 0 1 1-1 1L3.5 4.6a.8.8 0 0 1 0-1zm14.2 14.2a.8.8 0 0 1 1 0l1.8 1.7a.8.8 0 0 1-1 1l-1.8-1.7a.8.8 0 0 1 0-1z"/>
    <path class="moon" fill-rule="evenodd" d="M16.5 6A10.5 10.5 0 0 1 4.7 16.4 8.5 8.5 0 1 0 16.4 4.7l.1 1.3zm-1.7-2a9 9 0 0 1 .2 2 9 9 0 0 1-11 8.8 9.4 9.4 0 0 1-.8-.3c-.4 0-.8.3-.7.7a10 10 0 0 0 .3.8 10 10 0 0 0 9.2 6 10 10 0 0 0 4-19.2 9.7 9.7 0 0 0-.9-.3c-.3-.1-.7.3-.6.7a9 9 0 0 1 .3.8z"/>
  </svg>
</button>
<style>
  #themeToggle {
      border: 0,
      background: none;
    }
    .sun { fill: black;}
    .moon {fill: transparent;}

    :global(.dark) .sun { fill: transparent;}
    :global(.dark) .moon { fill: white; }
</style>
```

`import ThemeIcon from './ThemeIcon.astro';`

### Add CSS styling for a dark theme

in `global.css`

```
html.dark {
    background-color: #0d0950;
    color: #fff;
  }
.dark .nav-links a {
    color: #fff;
  }
```

### Add client-side interactivity

1. Add the following <script>

```
<script>
  const theme = (() => {
      if (typeof localStorage !== 'undefined' && localStorage.getItem('theme')){
         return localStorage.getItem('theme');
        }
      if (window.matchMedia('(prefers-color-scheme: dark)').matches){
        return 'dark';
      }
        return 'light';
  })();

  if (theme === 'light'){
    document.documentElement.classList.remove('dark');
  } else {
      document.documentElement.classList.add('dark);
    }

    window.localStorage.setItem('theme',theme);

    const handleToggleClick = () => {
        const element = document.documentElement;
        element.classList.toggle('dark');

        const isDark = element.classList.contains('dark');
        localStorage.setItem('theme',isDark ? 'dark' : 'light');
      }

      document.getElementById('themeToggle').addEventListener('click',handleToggleClick);
</script>

```

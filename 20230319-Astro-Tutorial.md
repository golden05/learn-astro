---
title: Astro Tutorial
tags: [Astro]
version: 2.1
---

# Build first Astro Blog

- setup your development environment
- create pages and blog posts for your website.
- build with Astro components
- Query and work with local files.
- Add interactivity to your site.
- deploy your site to the web

## setup

## Launch the Astro setup wizard

```
npm create astro@latest
npm run dev
```

## edit your home page : index.astro

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

## Create a new **.astro** file

under **src/pages/** create a new file named **about.astro**.

```
<body>
  <h1>About Me</h1>
  <h2>   and my new AStro site!</h2>

  <p>I am working through Astro's introductory tutrial.
</body>
```

# Add navigation links

```
<a href="/">Home</a>
<a href="/about">About</a>
```

# write first markdown blog post

create a new directory at **src/pages/posts/**, add a new file post-1.md
browser adding \*\*/posts/post-1

## content:

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

## Link to your blog posts

```
<ul>
  <li><a href='/posts/post-1'>Post 1</a></li>
</ul>
```

# Add dynamic content about you

## Define and use a variable

add Javascript in the **about.astro** in the frontmatter script
and use it in body section

```
---
const pageTitle = "About Me";
---
<title>{pageTitle}</title>

```

## write Javascript expressions in Astro

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

# Style your About page

## Style an individual page

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

## Use your first CSS variable

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

# Add a global stylesheet
create a new file at the location **src/styles/global.css** , global.css like this:


```
html {
background-color: red;
font-family: sans-serif;
}
```

in the **about.astro** , import to the frontmatter

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

## A Navigation component
- create a new folder **src/components** for components
- build an Astro component
- Replace existing HTML with a new Navigation component

a new component file  **Navigation.astro** like this:

```
<a href="/">Home</a>
<a href="/about">About</a>
```

## import and use Navigation.astro

```
---
## import Navigation from '../components/Navigation.astro'
---
```

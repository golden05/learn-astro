---
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

# setup

# Launch the Astro setup wizard

```
npm create astro@latest
npm run dev
```

# edit your home page : index.astro

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
    <h1>Astro</h1>
  </body>
</html>

```

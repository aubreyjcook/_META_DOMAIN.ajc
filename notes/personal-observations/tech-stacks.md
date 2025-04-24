# Tech Stacks

## Overview

This document describes some tech stacks that I have worked with or am interested in. It includes a brief description of each stack, its components, and any relevant notes or observations.

## Next.js

Next.js is the primary technology I now work with. It has numerous features and advantages. By default it is configured with react.js. As of this writing it has recently been updated to utilize the app router, which is a different approach that has a focus on server side rendering and app-like features. This is on contrast to the pages router, which was more focused on static site generation.

Next.js is suitable for server-side rendering (SSR) and static site generation (SSG).

Next.js also has direct integration for configurations with Typescript and Tailwind, both of which are technologies I now intend to use frequently.

As such, the base stack I generally use is:
- Next.js
 - Typescript
 - Tailwind CSS

After doing more research, there are a few other technologies that are valuable to use with this base stack explicitly. This includes shadcn/ui, which is a component library that is built on top of Tailwind CSS. Another technology is strapi, which is a headless CMS that can be used to manage content for Next.js applications.

These two technologies tend to be supplmentary to the base stack. Shadcn is highly relevant to Tailwind, and very powerful for customizing your UI with your own custom components, but on simpler projects it is less necessary and might be eschewed. Strapi is a headless CMS that can be used when self-hosting, and when the client requires CMS features to be able to update their own content, it is a good choice if you have self hosting solutions, which I consider to be the best option for development, and in addition to this, Strapi is open source and therefore more flexible than other CMS solutions. However, when a CMS is not a required feature it may be eschewed. Other CMS solutions may also be needed as opposed to Strapi, a good example is contentful or sanity, which are both SaaS solutions that are not self-hosted. Lastly, it's often the case that end users are familiar with or already using WordPress, and certain technologies allow the integration of WP with Next.js, such as WPGraphQL. This is a good option for clients who are already using WordPress and want to continue using it, but also want the benefits of Next.js.

This is the more expanded base stack I tend to rely on as such:
- Next.js
 - Typescript
 - Tailwind CSS
 - Shadcn/ui
 - Strapi (or other CMS)
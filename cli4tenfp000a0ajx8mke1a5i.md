---
title: "A comprehensive guide to Next.js 13 App Router - Part 1"
seoTitle: "A comprehensive guide to Next.js 13 App Router - Part 1"
seoDescription: "Master Next.js App Router: Create routes, layouts, handle dynamic paths, and build flexible web apps. Your comprehensive guide awaits!"
datePublished: Fri May 26 2023 17:06:25 GMT+0000 (Coordinated Universal Time)
cuid: cli4tenfp000a0ajx8mke1a5i
slug: a-comprehensive-guide-to-nextjs-13-app-router-part-1
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1685115232478/d225bf4b-d8b0-4338-a02d-bd42bfc3d0c4.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1685120625776/92712e8d-f2fc-4de4-8a99-92546a6b2ac1.png
tags: programming-blogs, javascript, reactjs, nextjs, wemakedevs

---

Next.js is a powerful framework for building server-side rendered React applications. One of its key features for routing is the App Router which was introduced in version 13, which allows you to define and organize routes in your Next.js application. In this comprehensive guide, we will explore the main concepts and techniques of Next.js App Router, providing you with the knowledge to effectively navigate and structure your Next.js applications. Let's dive straight into it 🚀

## Creating Routes

In Next.js version 12 we were used to create routes with a `js` / `jsx` or `ts` / `tsx` file inside the pages directory. But now in version 13 we define routes inside the `app` directory, with a `directory` (note: not a file). Each folder/direcotry represents a **route** segment that maps to a **URL** segment. Here is a visual for you to understand:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685116610914/3e59c95c-4955-4e7d-8851-fd65783da5a6.png align="center")

Now we can use a special file `page.js` to make our pages accessible. If you don't add any `page.js` file inside route folder we won't see anything on the route.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685117192651/cf115d37-29d9-4a2e-bcd5-f389de5d5043.png align="center")

Here the `analytics` route will not work because it hasn't any `page.js` inside it. This folder could be used to store components, stylesheets, images, or other colocated files.

Here is a code example of creating `app/page.js` page.

```javascript
// `app/page.js` is the UI for the `/` URL
export default function Page() {
  return <h1>Hello, Next.js!</h1>;
}
```

### Layout

A layout is UI that is **shared** between multiple pages. On navigation, layouts preserve state, remain interactive, and do not re-render.

To define a layout, you can start by creating a file called `layout.js` within the desired directory. This file will export a default React component that represents the layout. The component should accept a `children` prop, which will be populated with either a "child layout" or a "child page" during rendering.

For instance, let's consider a Typescript example of a `layout.tsx` file located at `app/dashboard/layout.tsx`:

```typescript
export default function DashboardLayout({
  children, // represents a page or nested layout
}: {
  children: React.ReactNode;
}) {
  return (
    <section>
      {/* Include shared UI here, such as a header or sidebar */}
      <nav></nav>
 
      {children}
    </section>
  );
}
```

In this example, the `DashboardLayout` component serves as the default layout for the dashboard section of your application. It receives the `children` prop, which dynamically renders the content within the layout. The `children` prop can be a page or another nested layout. Here is a visual for you to understand better:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685118757453/96cc337a-224b-469e-abbd-4127e2ca37bc.png align="center")

## Dynamic Routes

In Next.js App Router, dynamic routes allow you to generate routes based on dynamic data, making it easy to handle varying segment names and create dynamic paths. Whether you need to generate routes at request time or pre-render them during build time, dynamic segments provide the flexibility you need.

### Convention for Dynamic Segments

To create a dynamic segment, you can enclose a folder's name in square brackets, such as `[folderName]`. For example, `[id]` or `[slug]` can be used as dynamic segments.

Dynamic segments are then passed as the `params` prop to `layout`, `page`, `route`, and `generateMetadata` functions, allowing you to access the dynamic values within your components.

*Example: Blog routes*

For instance, if you have a blog where each post has a unique slug, you can define a route like this: `app/blog/[slug]/page.js`, where `[slug]` represents the dynamic segment for the blog posts.

```javascript
export default function Page({ params }) {
  return <div>My Post</div>;
}
```

In this example, the `Page` component receives the `params` prop, which contains the dynamic values from the URL. For the URL `/blog/a`, `params` would be `{ slug: 'a' }`, allowing you to render the appropriate content based on the dynamic segment.

### Generating Static Params

To generate routes at build time, you can use the `generateStaticParams` function in combination with dynamic route segments. This function allows you to retrieve data smartly and efficiently, reducing build times by automatically deduplicating fetch requests with the same arguments.

For example, let's consider an example of generating static params for the blog post routes:

```javascript
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json());
 
  return posts.map((post) => ({
    slug: post.slug,
  }));
}
```

In this scenario, the `generateStaticParams` function fetches the blog posts and generates the static params based on the retrieved data. This approach improves performance by pre-rendering the routes during the build process.

### Catch-all Segments

Dynamic segments can be extended to match subsequent segments by adding an ellipsis inside the brackets, such as `[...folderName]`. For example, `app/shop/[...slug]/page.js` would match `/shop/clothes`, `/shop/clothes/tops`, `/shop/clothes/tops/t-shirts`, and so on.

### Optional Catch-all Segments

Additionally, catch-all segments can be made optional by including the parameter in double square brackets, like `[[...folderName]]`. This allows the route to match with or without the catch-all segment. For example, `app/shop/[[...slug]]/page.js` would match `/shop`, as well as `/shop/clothes`, `/shop/clothes/tops`, and `/shop/clothes/tops/t-shirts`.

By utilizing dynamic routes and their variations, you can create powerful and flexible routing structures in your Next.js applications. Whether you need to handle dynamic data or generate routes dynamically, these features provide the necessary tools to build dynamic and engaging user experiences.

## Resources

Next.js Docs: [https://nextjs.org/docs/app/building-your-application/routing](https://nextjs.org/docs/app/building-your-application/routing)

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1685120335111/3f9a0cb0-0bb8-4f4b-8d48-a9a07c64acb6.png align="center")

## Hey! You reached the bottom. Thanks for the read 😍

Stay tuned for Part 2 of a comprehensive guide to Next.js App Router, where we'll explore more advanced techniques and features to further enhance your application's routing capabilities.

If you want to connect with me or want to have a chat, feel free to connect with me. I would like to talk to you!

\- [Twitter](https://twitter.com/niazmorshed_)  
\- [Github](https://github.com/NiazMorshed2007)  
\- [Linkedin](https://www.linkedin.com/in/niazmorsheddev/)
---
title: "Component Patterns in Next.js 13"
seoTitle: "The Recommended Component Patters in Next.js 13"
seoDescription: "Optimize web performance with Next.js. Learn efficient component patterns, placing client components at leaves, and composing server and client components."
datePublished: Mon May 22 2023 16:13:38 GMT+0000 (Coordinated Universal Time)
cuid: clhz1rd61000209lb5ucw7ppp
slug: component-patterns-in-nextjs-13
canonical: https://medium.com/@niazmorshed2007/component-patterns-in-next-js-13-b6439dc6324a
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1684752306472/526569bc-f36c-4837-9033-a1cf1bf1f87a.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1684771842592/04d247a0-a7c2-4453-b4d0-58fa7b5e9f3c.png
tags: javascript, web-development, reactjs, typescript, nextjs

---

When it comes to building robust web applications, performance optimization is a crucial consideration. Slow-loading websites can result in frustrated users and lost opportunities. Fortunately, Next.js, the popular React framework, offers a range of powerful features to tackle performance challenges head-on. In this article, we will dive into the latest patterns for Next.js 13, empowering you to design a highly efficient component tree and explore strategies for leveraging the strengths of client and server components. By the end, you'll be equipped with the knowledge and techniques to deliver blazing-fast web experiences.

### Client Components at the Outermost Leaves

To boost the performance of your application, Next.js recommends moving your client component to the outermost leaves of your component tree whenever possible.

For instance, suppose you have a Layout component that consists of static elements like a logo and links, alongside an interactive search bar that relies on state. Instead of making the entire layout a Client Component, it's preferable to extract the interactive logic into its Client Component, such as the &lt;SearchBar /&gt;. By doing so, the layout can remain as a Server Component, eliminating the need to send unnecessary component JavaScript to the client.

Here is a visual of how it's working:

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684758055364/d3991c97-b7d8-4fac-9b1f-ff11c7a541fe.png align="center")

Here is a code example of how Next.js recommends you extract your client component into the outermost branch.

```javascript
// SearchBar is a Client Component
import SearchBar from './searchbar';
// Logo remains a Server Component
import Logo from './logo';
 
export default function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  return (
    <>
      <nav>
        <Logo />
        <SearchBar />
      </nav>
      <main>{children}</main>
    </>
  );
}
```

By applying this approach, you ensure that only the necessary components are rendered on the client side, leading to improved performance and reduced resource overhead.

### **Composing Client and Server Components**

Server and Client Components can be combined in the same component tree.

Behind the scenes, React handles rendering as follows:

*Phase1:*

When the server kicks into action, React diligently renders all the Server Components, ensuring they're ready before being sent to the client. This rendering phase even covers Server Components nestled inside Client Components. During this server-side rendering, React wisely skips any Client Components it encounters, allowing them to shine later.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684770251640/c65a71a9-cb13-4f25-aa0a-e335c0f3a284.png align="center")

*Phase2:*

Now, enter the client-side realm. React takes the stage again, this time rendering the Client Components and seamlessly merging the work done on both the server and client sides. If a Client Component happens to house any Server Components, fear not. React ensures that the rendered content of those Server Components is placed precisely where it belongs within the Client Component.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1684770606804/eeed8c00-f7cd-4c71-813a-778adc23f6c5.png align="center")

### Nesting Server Components within Client Components

When it comes to integrating Server Components within Client Components in Next.js, there are certain restrictions to consider. Directly importing a Server Component into a Client Component is not supported, as it would require an additional server round trip, impacting performance.

***Unsupported Pattern: Importing a Server Component into a Client Component***

The following code pattern is not supported:

```javascript
'use client';

// This pattern will **not** work!
// You cannot import a Server Component into a Client Component.
import ExampleServerComponent from './example-server-component';

export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode;
}) {
  // ...
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>

      <ExampleServerComponent />
    </>
  );
}
```

***Recommended Pattern: Passing Server Components as Props to Client Components***

Instead, Next.js recommends utilizing React props to create placeholders, or "holes," for Server Components within Client Components. This ensures optimal performance and decoupling of rendering.

The recommended pattern:

```javascript
'use client';

// this pattern will work
import { useState } from 'react';

export default function ExampleClientComponent({
  children,
}: {
  children: React.ReactNode;
}) {
  // ...
  return (
    <>
      <button onClick={() => setCount(count + 1)}>{count}</button>

      {children}
    </>
  );
}
```

In this pattern, the ExampleClientComponent doesn't know the content of its children prop or that it will eventually be filled by the result of a Server Component. Its responsibility is solely to determine where the children should be placed.

The parent server component usage:

```javascript
// This pattern works:
// You can pass a Server Component as a child or prop of a Client Component.
import ExampleClientComponent from './example-client-component';
import ExampleServerComponent from './example-server-component';

export default function Page() {
  return (
    <ExampleClientComponent>
      <ExampleServerComponent />
    </ExampleClientComponent>
  );
}
```

By following this recommended pattern, Next.js empowers developers to compose components efficiently, improving performance and maintaining the separation of concerns between server-side and client-side rendering.

### Conclusion

In conclusion, Next.js provides powerful features and patterns to optimize performance in web applications. By strategically placing client components at the outermost leaves of the component tree, unnecessary JavaScript can be avoided, resulting in improved performance and reduced resource overhead. Additionally, Next.js allows for the seamless composition of client and server components, leveraging server-side rendering to ensure efficient rendering and merging of components on the client-side. When integrating server components within client components, it is important to follow the recommended pattern of using React props to pass server components as placeholders, enabling optimal performance and decoupling of rendering. By implementing these techniques and leveraging the strengths of Next.js, developers can deliver blazing-fast web experiences and enhance the overall performance of their applications.

### Like what you see? 💘

Feel free to follow me on Twitter, GitHub, and LinkedIn for more articles and insights on Next.js and other frontend development topics. Let's connect and stay updated on the latest trends and techniques in the world of web development!  
  
\- [Twitter](https://twitter.com/niazmorshed_)  
\- [Github](https://github.com/NiazMorshed2007)  
\- [Linkedin](https://www.linkedin.com/in/niazmorsheddev/)
---
title: "How to create a custom login page in NextAuth.js?"
seoTitle: "Step-by-Step Guide: Implementing Custom Login Page in NextAuth"
seoDescription: "Learn how to create a custom login page with NextAuth and improve your user experience. Our step-by-step guide makes it easy! #NextAuth #React"
datePublished: Thu Apr 13 2023 00:47:13 GMT+0000 (Coordinated Universal Time)
cuid: clgeehrxe00010ajx8lqs5v9c
slug: how-to-create-a-custom-login-page-in-nextauthjs
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1681315662606/5131d7b8-73be-4d85-8080-78489b8ba086.png
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1681347244243/aa617b19-c4ee-4562-bd68-d80f3490c735.png
tags: authentication, full-stack, reactjs, nextjs, nextauthjs

---

Before jumping into the main part let's talk about NextAuth.js first. NextAuth.js is a popular authentication library for Next.js applications that provides a simple and easy-to-use solution for handling user authentication and authorization. By default, NextAuth.js provides a built-in login page that can be customized with your branding and styling. However, if you want to create a completely custom login page that is tailored to your specific needs and design, you can do so by following the steps outlined in this tutorial.

### Step - 1: Initialize a Next.js app and install NextAuth.js

To get started let's first create a new Next.js app with `create-next-app` command. Open up your terminal in the folder where you want to be the project to be created.  
And then run the following command to create the project:

```bash
$ npx create-next-app nextAuthApp
$ cd nextAuthApp
```

With that set, now install the NextAuth.js package in our app to implement its functionality. You can install it by following the command:

```bash
$ npm install next-auth
```

### Step - 2: Add API route

To add NextAuth.js to a project create a file called `[...nextauth].js` in `pages/api/auth`. This contains the dynamic route handler for NextAuth.js which will also contain all of your global NextAuth.js configurations. The `[...nextauth].js` file will automatically handle all requests to `/api/auth/*` and also `(signIn, signOut, callback)` etc functions.  
  
Now let's add some code to the file! I'm only using GitHub provider for this tutorial, you can choose different providers depending on your need. Let's look at the code first and I'll explain it after that.

```javascript
import NextAuth from "next-auth"
import GithubProvider from "next-auth/providers/github"

export const authOptions = {
  providers: [
    GithubProvider({
      clientId: process.env.GITHUB_ID,
      clientSecret: process.env.GITHUB_SECRET,
    }),
    // ...add more providers here
  ],
  pages: {
    signIn: '/auth/signin'
  }
}

export default NextAuth(authOptions)
```

Here we created authOptions object where we initialized GitHub provider in the `providers` array. You can add Google, Facebook or other providers just like the same. For the `clientId` and `clientSecret`, go to Github &gt; developer settings &gt; OAuth Apps. And then register a new app and grab the Id and secret and put them in the `.env.local` file.  
Pay attention to the `pages` object in the `authOptions`. It will specify the custom pages for handling different auth functions provided by NextAuth. In this example, we specified the login page to be in the `/page/auth/signin` file. Similarly we can create custom pages for different functions (e.g. `signOut` , `newUser`) etc.

### Step - 3: Configure Shared session state

To be able to use `useSession` first we'll need to expose the session context, [`<SessionProvider />`](https://next-auth.js.org/getting-started/client#sessionprovider), at the top level of our application. Here is how we can do this 👇

```javascript
import { SessionProvider } from "next-auth/react"
export default function App({
  Component,
  pageProps: { session, ...pageProps },
}) {
  return (
    <SessionProvider session={session}>
      <Component {...pageProps} />
    </SessionProvider>
  )
}
```

Now we are all set to move on to our custom `signIn` page!

### Final Step: Create custom login page and connect functionality

Now we are on the final part. Let's look at the code first 👇

```javascript
import type { GetServerSidePropsContext, InferGetServerSidePropsType } from "next";
import { getProviders, signIn } from "next-auth/react"
import { getServerSession } from "next-auth/next"
import { authOptions } from "../api/auth/[...nextauth]";

export default function SignIn({ providers }) {
  return (
    <>
      {Object.values(providers).map((provider) => (
        <div key={provider.name}>
          <button onClick={() => signIn(provider.id)}>
            Sign in with {provider.name}
          </button>
        </div>
      ))}
    </>
  )
}

export async function getServerSideProps(context) {
  const session = await getServerSession(context.req, context.res,     authOptions);
  // If the user is already logged in, redirect.
  // Note: Make sure not to redirect to the same page
  // To avoid an infinite loop!
  if (session) {
    return { redirect: { destination: "/" } };
  }

  const providers = await getProviders();
  
  return {
    props: { providers: providers ?? [] },
  }
}
```

Take a closer look at the `getServerSideProps` function. We are using SSR to check the user's session and push the `providers` into the page `props`. With the `session` variable we are getting the info about the user (if he signed in or not). If we find that the user is signed in we redirect him back to the home page **(Note: Don't redirect to the same page, otherwise it will be in an infinite loop)** and if not we are injecting the providers into the props.  
  
Now in the `SignIn` function we are rendering our custom code. Notice the `{providers}` in the page props. Remember we pushed the providers in Server Side Rendering, it's that! In case if you have multiple providers, you can loop over through the `providers` object by making it an array with `Object.values(providers)`. And now add a onClick event to the button and pass the `signIn` function with the `provider.id` in the parameter.  
  
That's it! We've successfully created our custom login page! 🥳🥳

### Useful resources

Here is a video of [**Eddie Jaoude**](https://twitter.com/eddiejaoude) I found helpful about this topic. Even I learned this while I was contributing to [Linkfree](http://linkfree.io).  
  
\- [click here](https://github.com/EddieHubCommunity/LinkFree/pull/6016) to see the discussion on Github

%[https://www.youtube.com/watch?v=e2EKSJkXkqQ&t=768s] 

### Lastly,,,,,,

Thanks for reading 🥳🥳!!! Please drop out some feedback on my writing and like it if you like it. I really appreciate it 😀.  
  
I started open source a few weeks ago and I'll write blogs about my new discovery and learning while geeking out in open source. You can join my journey! 😁  
  
My Twitter: [niazmorshed\_](https://twitter.com/niazmorshed_)  
My Github: [NiazMorshed2007](https://github.com/NiazMorshed2007)
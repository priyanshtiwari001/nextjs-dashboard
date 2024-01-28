# [Next-js dashboard](https://nextjs.org/learn/dashboard-app/) short note that discuss in the chapter

## Chapter-3 Optimizing Fonts and Images

- Why optimize fonts?

  - **Cumulative Layout Shift** is a metric used by Google to evaluate the performance and user experience of a website. With fonts, layout shift happens when the browser initially renders text in a fallback or system font and then swaps it out for a custom font once it has loaded. This swap can cause the text size, spacing, or layout to change, shifting elements around it.

- How nextjs handle this?

  - Next.js automatically optimizes fonts in the application when you use the "next/font" module. It downloads font files at build time and hosts them with your other static assets. This means when a user visits your application, there are no additional network requests for fonts which would impact performance.

- [How to use font in your files](./app/ui/fonts.ts)

- The Image component

  - The <Image> Component is an extension of the HTML <img> tag, and comes with automatic image optimization, such as:

  - Preventing layout shift automatically when images are loading.
  - Resizing images to avoid shipping large images to devices with a smaller viewport.
  - Lazy loading images by default (images load as they enter the viewport).
  - Serving images in modern formats, like WebP and AVIF, when the browser supports it.

## Chapter-4 Navigating Between Pages

- Why optimize navigation?

  - To link between pages, you'd traditionally use the <a> HTML element, refresh the pages everytime when we navigate between pages.
  - To prevent this, In Next.js, you can use the <Link /> Component to link between pages in your application. <Link> allows you to do client-side navigation with JavaScript.

- Automatic code-splitting and prefetching:

  - To improve the navigation experience, Next.js automatically code splits your application by route segments. This is different from a traditional React SPA, where the browser loads all your application code on initial load.

  - Splitting code by routes means that pages become isolated. If a certain page throws an error, the rest of the application will still work.

  - Futhermore, in production, whenever <Link> components appear in the browser's viewport, Next.js automatically prefetches the code for the linked route in the background. By the time the user clicks the link, the code for the destination page will already be loaded in the background, and this is what makes the page transition near-instant!

## Chapter-5 Fetching Data

- Using Server Components to fetch data:

  - By default, Next.js applications use React Server Components.
  - Server Components support promises, providing a simpler solution for asynchronous tasks like data fetching.
  - You can use async/await syntax without reaching out for useEffect, useState or data fetching libraries.
  - Server Components execute on the server, you can query the database directly without an additional API layer.

- What are request waterfalls?

  - A "waterfall" refers to a sequence of network requests that depend on the completion of previous requests. In the case of data fetching, each request can only begin once the previous request has returned data.

- When might you want to use a waterfall pattern?

  - where you want waterfalls because you want a condition to be satisfied before you make the next request.
  - For example, you might want to fetch a user's ID and profile information first. Once you have the ID, you might then proceed to fetch their list of friends. In this case, each request is contingent on the data returned from the previous request.

- Parallel data fetching

  - A common way to avoid waterfalls is to initiate all data requests at the same time - in parallel.
  - In JavaScript, you can use the Promise.all() or Promise.allSettled() functions to initiate all promises at the same time
  - By using this pattern, you can:
    - Start executing all data fetches at the same time, which can lead to performance gains.
    - Use a native JavaScript pattern that can be applied to any library or framework. However, there is one disadvantage of relying only on this JavaScript pattern: what happens if one data request is slower than all the others?

  ## Chapter-8 Static and Dynamic Rendering

  - What is Pre-rendering?

    - Next.js pre-renders every page. This means that Next.js generates HTML for each page in advance, instead of having it all done by client-side JavaScript.

  - What is Static Rendering?

    - With static rendering, data fetching and rendering happens on the server at build time (when you deploy) or during revalidation[__Revalidation is the process of purging the Data Cache and re-fetching the latest data. This is useful when your data changes and you want to ensure you show the latest information__]. The result can then be distributed and cached in a Content Delivery Network (CDN).
    - Couple of benefit of static-rendering?

      - Faster Websites -> Prerendered content can be cached and globally distributed. This ensures that users around the world can access your website's content more quickly and reliably.
      - Reduced Server Load -> Because the content is cached, your server does not have to dynamically generate content for each user request.
      - SEO -> Prerendered content is easier for search engine crawlers to index, as the content is already available when the page loads. This can lead to improved search engine rankings.

    - **Static rendering is useful for UI with no data or data that is shared across users, such as a static blog post or a product page**. It might not be a good fit for a dashboard that has personalized data that is regularly updated.

  - What is Dynamic Rendering?

    - With dynamic rendering, content is rendered on the server for each user at request time (when the user visits the page).
    - There are a couple of benefits of dynamic rendering:

      - Real-Time Data -> Dynamic rendering allows your application to display real-time or frequently updated data. This is ideal for applications where data changes often.
      - User-Specific Content -> It's easier to serve personalized content, such as dashboards or user profiles, and update the data based on user interaction.
      - Request Time Information -> Dynamic rendering allows you to access information that can only be known at request time, such as cookies or the URL search parameters.
      - to use dynamic rendering, import unstable_noStore from next/cache, and call it the top of your data fetching functions.

## Chapter-9 Streaming

- What is streaming?

  - Streaming is a data transfer technique that allows you to break down a route into smaller "chunks" and progressively stream them from the server to the client as they become ready.
  - Two ways we can do that:

    - At the page level, with the loading.tsx file.
    - For specific components, with <Suspense>

  - What is loading.tsx?

    - loading.tsx is a special Next.js file built on top of Suspense, it allows you to create fallback UI to show as a replacement while page content loads.
    - Since loading.tsx is a level higher than /invoices/page.tsx and /customers/page.tsx in the file system, it's also applied to those pages.We can change this with Route Groups.
    - what is route groups?
      - Route groups allow you to organize files into logical groups without affecting the URL path structure. When you create a new folder using parentheses (), the name won't be included in the URL path

  - With <Suspense> component
    - Suspense allows you to defer rendering parts of your application until some condition is met (e.g. data is loaded). You can wrap your dynamic components in Suspense. Then, pass it a fallback component to show while the dynamic component loads.

- Deciding where to place your Suspense boundaries

  - Where you place your Suspense boundaries will depend on a few things:
  - How you want the user to experience the page as it streams.
  - What content you want to prioritize.
  - If the components rely on data fetching.
  - Take a look at your dashboard page, is there anything you would've done differently?

    - You could stream the whole page like we did with loading.tsx... but that may lead to a longer loading time if one of the components has a slow data fetch.
    - You could stream every component individually... but that may lead to UI popping into the screen as it becomes ready.
    - You could also create a staggered effect by streaming page sections. But you'll need to create wrapper components.
    - Where you place your suspense boundaries will vary depending on your application. In general, it's good practice to move your data fetches down to the components that need it, and then wrap those components in Suspense. But there is nothing wrong with streaming the sections or the whole page if that's what your application needs.

- Don't be afraid to experiment with Suspense and see what works best, it's a powerful API that can help you create more delightful user experience

## Chapter-10 Adding Search and Pagination

- Why use URL search params?

  - using URL search params, we will abe to manage the search state.
  - There are a couple of benefits of implementing search with URL params:
    - Bookmarkable and shareable url
    - Server-Side Rendering and Initial Load: URL parameters can be directly consumed on the server to render the initial state, making it easier to handle server rendering.

- These are the Next.js client hooks that you'll use to implement the search functionality:
  - **useSearchParams**- Allows you to access the parameters of the current URL. For example, the search params for this URL /dashboard/invoices?page=1&query=pending would look like this: {page: '1', query: 'pending'}
  - **usePathName** - Lets you read the current URL's pathname. For example, for the route /dashboard/invoices, usePathname would return '/dashboard/invoices'.
  - **useRouter** - Enables navigation between routes within client components programmaticall

## Chapter- 12 Mutating Data

- what is Server actions?

  - React Server Actions allow you to run asynchronous code directly on the server. They eliminate the need to create API endpoints to mutate your data. Instead, you write asynchronous functions that execute on the server and can be invoked from your Client or Server Components.

  - security is very important for web application, as they are vunerable to various threat. This is where Server Actions come in. protect us from various type of attacks, ensuring authorized access and etc.

  - Server Actions achieve this through techniques like POST requests, encrypted closures, strict input checks, error message hashing, and host restrictions, all working together to significantly enhance your app's safety.
  - An advantage of invoking a Server Action within a Server Component is **progressive enhancement** - forms work even if JavaScript is disabled on the client.

- Next.js with Server Actions

  - Server Actions are also deeply integrated with Next.js caching. When a form is submitted through a Server Action, not only can you use the action to mutate data, but you can also revalidate the associated cache using APIs like revalidatePath and revalidateTag.

- Revalidate and redirect

  - Next.js has a Client-side Router Cache that stores the route segments in the user's browser for a time.
  - Along with prefetching(pre-load the data in the background), this cache ensures that users can quickly navigate between routes while reducing the number of requests made to the server.
  - Since you're updating the data displayed in the invoices route,
    - you want to **clear this cache and trigger a new request to the server**. You can do this with the **revalidatePath** function from Next.js.
    - redirect helps to redirect to the path we mentioned.

  ## Chapter- 13 Handling Error

  - why we should call redirect outside of the try/catch block.

    - This is because redirect works by throwing an error, which would be caught by the catch block. To avoid this, you can call redirect after try/catch. redirect would only be reachable if try is successful

  - Error.tsx
    - The error.tsx file can be used to define a UI boundary for a route segment. It serves as a catch-all for unexpected errors and allows you to display a fallback UI to your users.
    - It accepts two props:
      - error: This object is an instance of JavaScript's native Error object.
      - reset: This is a function to reset the error boundary. When executed, the function will try to re-render the route segment.
  - notFound.tsx
    - While error.tsx is useful for catching all errors, notFound can be used when you try to fetch a resource that doesn't exist.

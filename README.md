# [Next-js dashboard](https://nextjs.org/learn/dashboard-app/)

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

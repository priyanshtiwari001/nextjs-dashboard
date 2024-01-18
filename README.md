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

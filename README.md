# PRPL HTML
An experiment implementing the [PRPL pattern](https://web.dev/apply-instant-loading-with-prpl/) with HTML files.

## See
Visit the [deployed site](https://prpl-html.netlify.app) to get a feel for how the mechanism works.

## Why
Typically this pattern is used to prefetch things like JavaScript bundles or JSON required to construct the DOM for the next page. I thought it might be interesting to boil it down further and prefetch the actual html of the next page.

## How it works
1. [Initial html file](/index.html) is loaded by the browser
2. After the page is loaded, we grab an achor tag that points to another html file and [fetch](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API) it
3. After that page is fetched, we save the html string in [local storage](https://developer.mozilla.org/en-US/docs/Web/API/Window/localStorage)
4. Still on the initial html file, we add an event listener to listen for clicks on that anchor tag
5. When a user clicks on that anchor tag, we tell the browser not to load it normally. Then, we take the `href` attribute and get the item we fetched earlier in local storage.
6. Now that we have the html string for the file we want to navigate to, we use the [DOMParser](https://developer.mozilla.org/en-US/docs/Web/API/DOMParser) interface to parse it into a document
7. Finally, we replace the current document body with the target document body

## Thoughts
- Using local storage needs further consideration:
  - Storage item size
  - Number of storage items
  - Read/write speed
  - Sites or users that disable local storage
- Should use a smarter technique to determine which links to prefetch and when to do it
- Should do the prefetch in a worker to offload work to another thread
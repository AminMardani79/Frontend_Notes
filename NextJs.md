### Pre-Rendering
#### SSG
* Reloading a page ---> Html + Javascript chunk
* Navigate to page via Link component ---> Javascript chunk + prefetched json file
* When we call a Link Component in a page in ssg mode a Javascript chunk and JSON file prefetch for that page and when we navigate to that page there is no further network request
* All these files are generated in the server at build time
* Javascript chunks make Html files that were sent before interactive and it calls hydration
* Instead of returning props we can return not found or redirect : { destination: false or true }
#### SSR
* Html file will generate when we request on each page.
* Visiting new page ----> Html ( will generate ) + Javascript chunk

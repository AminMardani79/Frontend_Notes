### Pre-Rendering
#### SSG
* Reloading a page ---> Html + Javascript chunk
* Navigate to page via Link component ---> Javascript chunk + prefetched json file
* When we call a Link Component in a page in ssg mode a Javascript chunk and json file prefetch for that page and when we navigate to that page there is no further network request
* All this files are generated in server at build time
* Javascript chunks make Html files that sent before interactive and it calls hydration

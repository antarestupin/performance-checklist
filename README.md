# Web Application Performance Checklist

[![Awesome Checklist](https://img.shields.io/badge/Awesome-Checklist-blue.svg)](http://checklist.yingjiehu.com/)

Performance is crucial in today's web applications. A slow app feels buggy to the users and make them flee it.

Although performance is such an important topic, there are so many optimization vectors that it's easy to forget some of them.
The intent of this checklist is to gather performance practices to apply when developing a web application.

Contributions and stars are welcome 🙏

While you apply these practices, you should always keep the following rules in mind:

- Don't optimize too early
- Don't let performance ruin productivity
- Always check an optimization is efficient by measuring performance before and after it


## Table of Contents

- [Set your objectives](#set-your-objectives)
- [Think about performance when building your tech stack](#think-about-performance-when-building-your-tech-stack)
- [Frontend](#frontend-1)
- [Backend](#backend-1)
- [Database](#database-1)
- [Network and Infrastructure](#network-and-infrastructure)
- [Measuring](#measuring)
- [Misc](#misc)


## Set your objectives

- [ ] Define the **performance metrics and objectives** that are important to your business
    - The [RAIL model](https://developers.google.com/web/fundamentals/performance/rail) is generally a good model to start with.
    - If speed is an advantage you want to have against competitors, know that users usually will feel you are faster if you are at least 20% faster than them.
- [ ] Plan out a **loading sequence**; this way you can define early what is really important in your content, what to load first and what to load later
- [ ] Make a **performance budget**
    - The [performance budget calculator](http://www.performancebudget.io) is useful to estimate your budget depending on the performance you want to obtain. [This one](https://codepen.io/bradfrost/full/EPQVBp) is nice too.
    - Remember that this budget takes compression in account.
    - Currently the recommended budget is [max. 170kb gzipped](https://infrequently.org/2017/10/can-you-afford-it-real-world-web-performance-budgets/), but it really depends mostly on your user-centric objectives

## Think about performance when building your tech stack

Before beginning to build your app, you should have some preparation in order to ease your work on performance later.

### Frontend

- [ ] If choosing between SPA frameworks, **take in account features like server side rendering**; these features will be hard to add later
- [ ] Take in account **how much every library / framework will take on your performance budget**; don't use too much of them
- [ ] **Make sure you need custom fonts** before using them
    - [This article](https://hackernoon.com/web-fonts-when-you-need-them-when-you-dont-a3b4b39fe0ae) can help you
- [ ] Consider technologies like **[AMP](https://www.ampproject.org/fr/)** and **[Instant Articles](https://instantarticles.fb.com/)**, but be aware of their pros and cons
    - Keep also in mind these solutions are not mandatory to obtain correct performances.

### Backend

- [ ] When choosing a web framework / library / language, take in account the following points:
    - How **fast** is the library / underlying language (but be aware that benchmarks are usually biased)?
    - How easy will it be to handle **concurrency**?
    - Does it allow an **efficient resources management**, e.g. using a connections pool or an event loop?

### Database

- [ ] Make sure you **use the right DBMS** for your needs
    - Usually a relational database will cover most needs, but in some cases a NoSQL database may be a better fit.
    - If hesitating between NoSQL solutions, [this comparison](https://kkovacs.eu/cassandra-vs-mongodb-vs-couchdb-vs-redis) may help you.


## Frontend

### Optimize images

Images represent in average ~60% of a page's weight, thus it's an important part to optimize.

- [ ] Use **WebP compression** format for browsers that accept it
- [ ] Use **responsive images** with `img`'s' `srcset` and `size` attributes
- [ ] **Optimize** manually **important images** or script their optimization
- [ ] **Lazy load** images

### Reduce code size

- [ ] **Minimize** the source code
- [ ] Use **tree shaking** (e.g. with [webpack](https://webpack.js.org/)) to remove unused code
- [ ] If your bundled code file is too big, use **code splitting** to load only what's needed first and lazy load the rest

### Reduce number of requests

- [ ] Replace third parties components (like sharing buttons, maps...) by **static components**
    - Tools like the [Simple sharing buttons generator](https://simplesharingbuttons.com) can help you.
- [ ] Cache requests client side using **service workers**
- [ ] Bundle common images using **CSS sprites**

### Prepare next requests

You can use prefetching to prepare the browser to next requests and make them faster or even instant. [This article](https://css-tricks.com/prefetching-preloading-prebrowsing/) is a few old but explains well the following techniques.

- [ ] Use **dns-prefetch** to resolve the domain of services you may need
- [ ] Use **preconnect** to do DNS lookup, TCP handshake and TLS negociation with services you know you will need soon
- [ ] Use **prefetch** to request specific resources that are likely to be needed soon, like images and scripts
    - This technique makes an especially good combination with lazy loading.
- [ ] Use **preload** to request specific resources that will be needed in the current page, e.g. `<script>` tags at the end of the body
    - The difference between `prefetch` and `preload` is explained [here](https://medium.com/reloading/preload-prefetch-and-priorities-in-chrome-776165961bbf).
- [ ] Use **prerender** to request and prerender pages that are very likely to be visited soon, like homepage or user dashboard
    - Be aware that this technique is quite heavy, make sure you know what you are doing before applying it.

### Optimize time to rendering

Ideally the critical code should fit in 14KB in order to be server in the first TCP roundtrip ([why 14KB?](https://tylercipriani.com/blog/2016/09/25/the-14kb-in-the-tcp-initial-window/)). These techniques help to achieve this goal.

- [ ] **Inline critical CSS** in the `<head>` of your pages
    - Tools like [CriticalCSS](https://github.com/filamentgroup/criticalCSS) and [critical](https://github.com/addyosmani/critical) can help you defining your site's critical CSS
- [ ] If critical CSS is inlined, you can then **defer the CSS files loading**
    - [loadCSS](https://github.com/filamentgroup/loadCSS) can help you achieving that
- [ ] When the CSS files are loaded, **set a cookie to avoid using inlined critical CSS anymore**; this way you will benefit from browser cache
    - More explanations on this technique [here](https://gomakethings.com/inlining-critical-css-for-better-web-performance/#what-about-browser-caching)
- [ ] **Defer scripts execution**, especially social media buttons and ads
- [ ] **Defer fonts loading**; the technique is explained [here](https://www.sitepoint.com/improving-font-performance-subsetting-local-storage/)
- [ ] Use **server side rendering** if making an SPA

### Optimize fonts

- [ ] **Subset your fonts** using [Font Squirrel's font generator](https://www.fontsquirrel.com/tools/webfont-generator)
- [ ] Use **WOFF2** with fallback to WOFF and OTF

### Make animations smooth

- [ ] Prefer animating using CSS' **transform** and **opacity**; more explanations [here](https://www.html5rocks.com/en/tutorials/speed/high-performance-animations/)
- [ ] If animating with JS, use **requestAnimationFrame** instead of `setInterval`
- [ ] **Avoid animating during high network activity**; for example wait your page is fully loaded

### User perceived performance

User perceived performance is often disregarded but can be more important than actual performance. These techniques allow to improve it.

- [ ] You should use a **loader only on "long" / "heavy" tasks**, i.e. tasks the user can imagine they are heavy (e.g. account creation)
- [ ] Instead you can use **animations** to illustrate the transitions following user's action, for example transition from a page to another
- [ ] Show **app shell before content** if needed; more explanations [here](https://developers.google.com/web/fundamentals/architecture/app-shell)
- [ ] If using JPG images, you can use **progressive JPGs** to improve their loading perception
- [ ] Blur version or background color of lazy loaded images until they're loaded
- [ ] If not using especially JPG, you can **replace your images by cheaper components** until they are loaded
    - You can replace an image by a canvas filled with its main color
    - You can replace an image by a very lightweight, blurred version of it. This efficient technique is explained in [this article](https://code.facebook.com/posts/991252547593574/the-technology-behind-preview-photos/) from Facebook.
- [ ] Make an **optimistic UI** to make some interactions feel instant; a quick explanation can be found [here](https://uxplanet.org/optimistic-1000-34d9eefe4c05)


## Backend

### General

- [ ] Provide **batch queries / transactions** instead of making the client send multiple requests
- [ ] **Identify & optimize** slow resources
- [ ] **Parallelize** slow tasks
- [ ] Use relevant **data structures**
- [ ] Don't overuse **serialization**
- [ ] **Generate static content when deploying** so that it will be computed only once

### Cache

- [ ] Use **HTTP cache**; the different caching techniques are explained in [this guide](https://developer.mozilla.org/docs/Web/HTTP/Caching)
- [ ] Consider using **ESI** if your app is not a SPA
- [ ] **Cache calls to other services** using Redis, a reverse proxy...
- [ ] **Cache data** slow to compute and **memoize** slow functions

### Don’t loose time with non urgent tasks

- [ ] **Defer tasks** to workers using a queue or use event based patterns like Event Sourcing
- [ ] Use **UDP** for immediate but not vital tasks like logging

### Don’t loose time with errors

- [ ] **Fail fast** by validating request inputs as soon as possible
- [ ] Use the **circuit breaker** pattern to avoid waiting timeout when needing another service
- [ ] On sensible resources, **detect suspicious requests as attacks before actually handling them**; attacks can cause heavy resources consumption
    - For example, detect a login request as part of a brute force attack before fetching user data from the database.


## Database

- [ ] First, use **indexes** smartly
    - [Use the Index, Luke](http://use-the-index-luke.com) provides a complete guide on this subject for common RDBMS.
 [here](http://use-the-index-luke.com/sql/partial-results/fetch-next-page)
- [ ] **Tune** the DB; tools like [MySQLTuner](https://github.com/major/MySQLTuner-perl) and the experimental [OtterTune](https://github.com/cmu-db/ottertune) can help you
- [ ] **Identify & optimize critical and slow queries** (e.g. code that produces n+1 queries)
- [ ] If using pagination, **use last row instead of `offset`** as a starting point; more explanations [here](http://use-the-index-luke.com/)
- [ ] Once you are sure the used DBMS is the good one for your needs, **take advantage of its advanced features** (e.g. materialized views in Oracle, hyperloglogs in Redis...)
- [ ] **Don’t use ORM for complex queries**, unless you know what you’re doing


## Network and Infrastructure

- [ ] Serve static content using a **CDN** to shorten the distance between the client and the server
    - When using your CDN take into consideration features like HTTP/2 support, compression...
- [ ] Deploy your app on **several datacenters**, also to shorten the distance between the client and the server
- [ ] Serve resources compressed using **Brotli** if it's supported, **Gzip** otherwise
- [ ] Compress resources that are rarely changed using **Zopfli**
- [ ] Use **HTTP/2** and its features like **server push** and enable **HPACK** to compress HTTP headers
- [ ] Use **OCSP stapling** to fasten TLS shaking
- [ ] **Avoid redirects** as they increase the number of needed requests
- [ ] If using a microservices architecture, **bring services needing each other often closer**, ideally in the same machine
    - Kubernetes' pods can help achieve this goal


## Measuring

- [ ] Measure **server side** performance; this is usually already done by the web framework
- [ ] Measure **client side** performance; tools like [Web Page Test](http://www.webpagetest.org) can help you
- [ ] Measure client side performance **by country**; results may hugely differ from one to another
- [ ] Use tools like [Lighthouse](https://developers.google.com/web/tools/lighthouse/) to **audit** your site
- [ ] **Load test** your servers as they probably won't have same perfs under 10rps and 1000rps
- [ ] Keep track of **queries to the databases** to ease slow queries discovery


## Misc

- [ ] Keep your **dependencies up-to-date** as their performance is often improved by their maintainers

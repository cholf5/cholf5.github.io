---
author: 'Cholf'
title: 'No Image in Twitter Share Card? It Might Be Blocked by robots.txt'
date: '2025-09-08'
summary: "Last night, I suddenly discovered that the preview image in the card [YearProgress.org](https://www.yearprogress.org) was missing when I shared it on Twitter."

tags:
- twitter
- twitter-card
- robots.txt
- open-graph
- web-development
- web-crawling
---

## Problem
Last night, I suddenly discovered that the preview image in the card [YearProgress.org](https://www.yearprogress.org) was missing when I shared it on Twitter. This is really frustrating, because the original purpose of this website was to generate a progress bar preview when sharing on Twitter. If there is a problem here, the website's usefulness is almost zero.

Strangely, the preview on [Twitter Card Validator](https://threadcreator.com/tools/twitter-card-validator) was normal. This misled me for a while, leading me to suspect my OG information was incorrect or there was a problem with Twitter's server caching mechanism.
> Later, I learned that the preview on this website was normal because it crawls in real time and ignores robots.txt restrictions.

I asked the AI ​​about this, but it also gave me a lot of misinformation.

I then searched Twitter for the hashtag #YearProgress, sorted by time, and found the most recent preview to see how others were doing it. It turned out I had done nothing wrong; the approach was essentially the same.

Then I remembered that Twitter also has an official Card Validator, so I searched Google for `Twitter Card Validator site:x.com` and, sure enough, I found it: https://cards-dev.x.com/validator

I put my link in there, and the log clearly indicated a problem with my robots.txt file:

INFO: Page fetched successfully INFO: 35 metatags were found
INFO: twitter:card = summary_large_image tag found
INFO: Card loaded successfully

WARN: The image URL https://www.yearprogress.org/api/og?year=2025&day=251&lang=en specified by the 'twitter:image' metatag may be restricted by the site's robots.txt file, which will prevent Twitter from fetching it.

## Reason

- Twitter's crawler (`Twitterbot`) respects robots.txt.
- If your robots.txt file contains the following:

User-agent: *

Disallow: /api/


or similar rules, Twitter will not be able to retrieve images from `/api/og?...`.
- The Validator preview function works because it simulates a browser request directly, unaffected by robots.txt restrictions. However, when the card is actually rendered, Twitterbot will be blocked.

## Solution
Once you know the cause, the solution is simple: simply allow `/api/og` in robots.txt.

The final robots.txt file looks like this:
```
User-Agent: *
Allow: /api/og
Disallow: /api/
Disallow: /private/
Disallow: /_next/
Disallow: /admin/

User-Agent: Twitterbot
Allow: /api/og

User-Agent: facebookexternalhit
Allow: /api/og

Sitemap: https://www.yearprogress.org/sitemap.xml
```

## Postscript

I never imagined that images like `og:image` would be blocked by the rules in `robots.txt`. This is actually quite unreasonable, because `robots.txt` is a gentleman's agreement, and it should be followed when necessary. Since the website owner clearly provides this link in the `og:` file, crawling is definitely allowed. Complying at this point seems ungentlemanly and pedantic.

Of course, it is also possible that it is not pedantic, but the robot that crawls OG is coupled with Twitter's own crawler that crawls data, resulting in unexpected results here.
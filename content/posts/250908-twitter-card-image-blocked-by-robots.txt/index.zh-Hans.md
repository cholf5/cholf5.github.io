---
author: '周尔复'
title: '推特分享卡片没图？可能是 robots.txt 阻止了抓取'
date: '2025-09-08'
summary: "昨晚发现 [YearProgress.org](https://www.yearprogress.org) 在X上分享时，卡片的预览图没了。这网站的初衷就是在X上生成进度条预览，这是致命问题。"
tags:
- twitter
- twitter-card
- robots.txt
- open-graph
- web-development
- web-crawling
image: "/posts/250908-twitter-card-image-blocked-by-robots.txt/feature.webp"
---

## 问题
昨天晚上突然发现 [YearProgress.org](https://www.yearprogress.org) 在推特上分享时，卡片里的预览图没了。这可要了亲命了，因为这个网站创建的初衷就是为了在推特上分享时有能生成进度条预览的。这里出问题，这个网站的作用就几乎等于零了。

但是奇怪的是，在 [Twitter Card Validator](https://threadcreator.com/tools/twitter-card-validator) 预览是正常的，这个误导了我挺久，让我一度怀疑是不是我的 OG 信息有误，或推特服务器的缓存机制有问题。
> 后面才知道这个网站的预览之所以正常，是因为它是实时抓取，且忽略了 robots.txt 的限制。

问了 AI 也是一堆误导。

后面在推特上搜 #YearProgress 这个 Hashtag，按时间排序，找到最近能预览的，看一下人家是怎么做的。结果看来看去，我没做错什么，做法基本一模一样。

后来我想起来推特官方也有一个 Card Validator，就在 Google 里搜 `Twitter Card Validator site:x.com`，果然搜到正主了：https://cards-dev.x.com/validator

把我的链接放进去，日志明确的告诉我是 robots.txt 出了问题：
```
INFO: Page fetched successfully INFO: 35 metatags were found
INFO: twitter:card = summary_large_image tag found
INFO: Card loaded successfully

WARN: The image URL https://www.yearprogress.org/api/og?year=2025&day=251&lang=en specified by the 'twitter:image' metatag may be restricted by the site's robots.txt file, which will prevent Twitter from fetching it.
```

## 原因

- 推特的爬虫（`Twitterbot`）会遵守 robots.txt。
- 如果 `robots.txt` 里有：
  ```
  User-agent: *
  Disallow: /api/
  ```
  或类似规则，那么 Twitter 就拿不到 `/api/og?...` 的图。
- Validator 能预览，是因为它直接模拟浏览器请求，不受 robots.txt 限制；但实际卡片渲染时，Twitterbot 会被挡住。

## 解决方法
知道原因后，解决起来就很简单了，在 robots.txt 里放行 `/api/og` 就好了。

最终的 robots.txt 是这样的：
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

## 后记

我从来没想过 `og:image` 这样的图会被 `robots.txt` 的规则阻拦。这其实相当不合理，因为 `robots.txt` 本来就是君子协议，在该遵守时遵守就可以了。网站主人明确在 og 里提供了这个链接，那一定是允许你抓取的，这时候再遵守，不像君子，倒像是迂腐了。

当然也有可能不是迂腐，只不过抓取 OG 的机器人，和推特本身爬取数据的爬虫耦合在了一起，导致这里出现意想不到的结果。

---
layout: post
title: setting github pages
date: 2014-11-02 23:42:24
categories: github
---

There are only two steps to setup your personal blog with your own domain.

* Create a CNAME file under the root of your repos(e.g. https://github.com/sicongtang/sicongtang.github.io/blob/master/CNAME), in hexo or octopress blog system, just create a CNAME file under source folder. This will automatically deploy CNAME file to your repository's root directory.

* Then you need to configure a custom subdomain with your DNS provider. Creating two A records helps you to use custom apex domains. In addition, if you configure both an apex domain (e.g. sicongtang.me) and a matching www subdomain (e.g. www.sicongtang.me), GitHub's servers will automatically create redirects between the two.

<!--more-->

![Alt text](https://farm4.staticflickr.com/3953/15506283948_9c7c8d3c81_z.jpg "net_dns_config_githubblog_page")

References:

1. https://help.github.com/articles/setting-up-a-custom-domain-with-github-pages/
2. https://help.github.com/articles/tips-for-configuring-an-a-record-with-your-dns-provider/
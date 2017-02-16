---
layout: post
title: Things you need to know about Cookie optimization
date: 2016-06-01 00:04
tags: 
  - Web
  - Optimization
slug: things-you-must-know-about-cookie-optimization
---

HTTP cookies are widely used for authentication and personalization. Cookies are stored in HTTP request headers, transmitted between browsers and servers. It's very important for us to keep the size of cookies as low as possible to minimize the impact on the user's response time, especially for a large-scaled system.

## Use `localStorage` for storing the data that your server doesn't care about

For example, if you happened to be the owner of GitHub again, and want to make sure when a user is editing an issue page, the content in the text editor won't disappear if he refreshes the page. In this case, you should use `localStorage` to store temporary data in the browser. Don't put this kind of stuff in cookies ever.

## Host static resource from a different domain to avoid unnecessary cookie traffic

It's a bad practice to host your static resources, i.e. assets, images and other content, under the same domain as your main site. If you really did, every HTTP request users made to the static resources will carry a copy of the cookies of under your main domain, which is totally a waste of the bandwidth.

For instance, if you happened to own `github.com` and need to find a domain name to put the logo image of the site, you should avoid putting it either at `github.com/images/logo.gif` or at `images.github.com/logo.gif`. If users visit either of the above URLs, they will put a copy of the cookies of `github.com` inside the HTTP header. A good way to deal with this is to get a new domain for serving static files. For example, GitHub uses `githubusercontent.com` to serve user uploaded images.

Also, it is notable that browsers have maximum concurrent get request to the same domain, so serving static resources from a different domain also improves the loading speed of the web page more or less.

## Keep your Cookies short

To keep the cookies short, both the key and value should be as short as possible. For example, it's a good practice to use `0` and `1` for boolean values rather than `false` and `true`, and use `u` as the key for storing username rather than use `username`, which is too long.
---
layout: post
title: "Run shadowsocks in a router with OpenWRT to unblock the Chinese network block"
date: 2015-04-22 14:35
categories: Hacking
tags:
  - OpenWRT
  - Shadowsocks
  - Chinese GFW
slug: Run-Shadowsocks-on-OpenWRT
---

![Shadowsocks](/images/Shadowsocks_logo.png)

Nowadays, Shawdowsocks has become a lot more popular in China. We use it to break though the Chinese network block so that we can visit Google without blocking. If you're living in China and have the demand of visiting websites like Google, Twitter, YouTube and BBC, Shadowsocks is a great choice to build your proxy server. However, if you're not living in China but still have the difficulty playing online games as the ping is too high and connection is unstable, you can still try Shadowsocks as it may bring you a more stable network environment as long as the performance of your remote server is not too bad.

Shadowsocks client has supported a lot types of clients. For installation and configuration of Shadowsocks on a remote server and on the local client, please visit [Shadowsocks.org](http://shadowsocks.org/en/index.html). However, Shadowsocks can not be installed on some devices such as non-jailbroken iOS devices(has a app store version but only the built-in browser works), Xbox and PS4. If you want to use shadowsocks on those devices, there are two ways to do it.

1. Use [Privoxy](www.privoxy.org) to turn the SSH tunnel (SOCKS5) provided by shadowsocks into a HTTP proxy, then configure the HTTP proxy on the devices like iPhone or Xbox.

2. Install shadowsocks on a router and build a transparent proxy, so that any device connected to the router was proxied by the remote server.

## Installation of Shadowsocks

Firstly, I assume that openWRT has been succesfully installed on the router, you know how to login to the router via ssh and you know how to config a Shadowsocks client.

If not, please visit [OpenWrt – First Login](http://wiki.openwrt.org/doc/howto/firstlogin).

Firstly, install `shadowsocks-libev-spec` and `luci-app-shadowsocks-spec` in order.

    opkg update
    opkg install shadowsocks-libev-spec
    opkg install luci-app-shadowsocks-spec

`shadowsocks-libev-spec` is the core Shadowsocks package for OpenWRT and `luci-app-shadowsocks-spec` provides a web interface for Shadowsocks so that we can just fill out the configuration form to setup the Shadowsocks service.

After installing the package above, we may find **Shadowsocks** under the **Services** tab:

![OpenWRT Shawdowsocks](/images/openwrt.png)

Notice that **Proxy Method** can be **Global Proxy** if you want all traffic go though the remote server. However, if you want to bypass all the traffic to Chinese IPs, you may choose **Ignore List**, which was defined at `/etc/shadowsocks/ignore.list` in default.

Fill out the configuration form and choose **Save & Apply**, then Shadowsocks will run as a deamon in the background.

## Solution to DNS spoofing

The GFW(Great Firewall of China) has multiple methods to block network. One of them is the DNS spoofing, also named DNS Cache Poisoning. The GFW intercepts all UDP traffic on port 53(traffic on UDP port 53 usually means that it is a DNS lookup) and quickly, ahead of the real DNS server, send a fake response to the user when it finds the domain keyword matches the GFW blacklist. By this way the GFW may block foreign websites by faking the results of DNS queries that was made under it, which means that clients can not get the real IP address of the domain name, so of course they can not reach to the real server.

To get rid of the DNS sppofing we need [ChinaDNS](https://github.com/clowwindy/ChinaDNS), which creates a UDP DNS Server at a certian local port. ChinaDNS has a built-in IP blacklist. When a DNS query was made by the client to the ChinaDNS server, it looks up from the upstream DNS server. If the result matches an entry from the IP blacklist, ChinaDNS would regard it as a fake IP address and would wait for the result from the real DNS server.

The installation of ChinaDNS is pretty easy.

    opkg install chinadns
    opkg install luci-app-chinadns

After the installation of the two above pcakges. **ChinaDNS** may appear under the **Services** tab.

To get ChinaDNS to work, we have to forward DNS querys to a local port of the router. And run ChinaDNS on the same port.

1. Set up DNS forwardings under **Network - DHCP and DNS**, take the port whatever you like as long as it was not occupied, e.g. 5353.

2. Under **Services - chinadns**, set **Port** the same as the port you set in setp 1. Then set the **Upstream DNS Server**, **114.114.114.114** and **8.8.8.8** were recommended here. (As for me, I used my private DNS server running on my VPS)

3. Cllick **Save & Apply** and get ChinaDNS run.

Done! Now you may [check your public IP address](https://www.google.com/search?q=ip) to see if it is the same as your shadowsocks server.

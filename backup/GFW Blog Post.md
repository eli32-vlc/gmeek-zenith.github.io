![Banner](https://files.catbox.moe/n1u6by.png)


In this blog post, we are going to talk about how the GFW (Great Firewall) functions, common tools to bypass it, and explain how China's international internet functions.

### How does the GFW function?

We don't know fully how GFW works but there are research articles from China on how to build GFW. We can make some educated guesses based on the projects around us. The GFW works by doing DPI (deep packet inspection)—that’s how most government censorship firewalls work. It inspects your internet packets looking for blocked sites, prohibited content, etc. Then it responds back with either a server reset error or a DNS poisoning attack.

Research Articles (I am forced to put these articles on the blog.)

Article 1: [Link](https://github.com/net4people/bbs/issues/435)

Article 2: [Link](https://github.com/net4people/bbs/issues/437)

Article 3: [Link](https://github.com/net4people/bbs/issues/434)

### How does the GFW detect VPNs?

DPI can look for common patterns caused by common VPN protocols like OpenVPN or WireGuard. They have distinct patterns and use unique ports in their traffic. OpenVPN or WireGuard are most likely long-lived connections—in human terms, these connections are being created and used indefinitely. The GFW will get suspicious of that.

### How does the GFW detect common proxy protocols?

There are many detected attempts of active probing done by the GFW, where it sends a request to the server, and if the server responds like a typical proxy server (e.g., asking for auth, etc.), it will get blocked. The GFW can also look for TLS client and server TLS fingerprints—if they match, you are getting blocked.

### Do ShadowSocks work in these times?

ShadowSocks is essentially an encrypted version of SOCKS that can kind of be disguised as HTTPS traffic—not fully, and we'll get to that in a moment. Due to some exploits, and since late 2021, the GFW uses real-time heuristic-based methods analyzing encrypted traffic fingerprints (e.g., byte patterns, ASCII ratios) to block ShadowSocks with low false positives. More details here: [Link](https://gfw.report/publications/usenixsecurity23/en/). The GFW also targets cloud providers more than normal IP addresses. It’s more common that VPNs and proxies are blocked during sensitive times, normally around June. Also, I need to mention that ShadowSocks's author has been arrested by the Chinese authorities and has been "drinking tea" since then (if you don’t get that, it’s fine). That might be a rumor but the project has been taken down due to authority issues, he/she said that him self not me.

### More recent protocols to bypass the GFW

If you need an installation guide, please consult my censorship bypass guide [here](https://2305878273.7844380499.cfd/).

#### NaiveProxy

The newest one and has the lowest detection because it uses normal HTTPS/2 traffic and the Chromium browser stack, which makes the TLS client server fingerprint look like Chrome’s. The downside is the clients—there is no client on iOS (at least any I trust), so you have to proxy through your desktop or use Android.

#### VMESS

Part of the V2Ray suite. It's an older protocol developed by the V2Ray Project. It supports custom encryption and can be used basically anywhere (iOS, etc.). The downside is that it’s easier to detect and has slower speed due to encryption overhead.

#### VLESS

Part of the V2Ray suite but developed by V2Fly. It **doesn't** have built-in encryption, but it relies on TLS, which is harder to detect. You can also pair VLESS with Reality and XTLS. It’s more lightweight and supports pretty much every single client. It’s also faster than VMESS due to being lightweight.

#### Trojan

Mimics HTTPS traffic. Not really maintained anymore. No new updates is being pushed through Github. Pretty much dead—VLESS is better anyway. Or use NaiveProxy.

#### Hysteria 2

Hysteria 2 is a proxy protocol designed for speed. It uses UDP and QUIC instead of slower TCP. It can also force sending multiple packets when the network is congested. It’s better for gaming and low-latency applications, but the downside is that it can be blocked in places like schools where only TCP is allowed.

### What are some ways to connect to these proxy servers?

![Connecting to the proxy server](https://files.catbox.moe/8nohs6.png)

Let me walk you through each method one by one.

### Direct connection

Pretty easy to understand—it uses NaiveProxy or V2Ray suite to connect to the proxy server directly. The upside is that it is very simple to run and is the cheapest to run. The downside is that it can be very, very slow during high peak hours like 10 p.m. in China, and it can be very easily blocked if they detect it's a proxy server.

#### IPLC Forwarding

First, we need to understand what IPLC is. IPLC stands for International Private Leased Circuit. Instead of sharing the same international output where network congestion happens, you have a dedicated cable to route your traffic through. Put simply, it’s like having your own lane on the highway, and it’s only reserved for you. Because it’s private, it has no GFW and can’t be blocked unless the owner gets arrested or something happens. It’s like a local connection on your home router—when the network is very slow because of congestion, your local network is still blazing fast when doing network transfers between local devices.

#### Normal Forwarding

Having an inbound server in China means a more stable connection for the customer. Let’s say you run an airport (proxy server company). You don’t have to make the customer change servers every time it gets blocked—you can do this on your end. It also means the customer or you can use ShadowSocks, and it might be more widely compatible. Unless the inbound server uses a custom international output that still has the GFW but is custom for you, like CN GIA2 (Global Internet Access). During peak hours, the speed is still going to be low. Also, it’s more expensive to run and risky due to having a server in China.

> Grammar and spelling fixed by AI but the text is written by Zenith.

+++
title = "Quick terminal-based DNS switching"
date = "2019-10-19T14:53:23.322Z"
authors = ["stanleynguyen"]
cover = "/posts/dns-command/img/cover.png"
tags = ["dns", "privacy", "terminal", "network"]
keywords = ["dns", "privacy", "terminal", "bash", "network", "nameserver"]
description = "A quick guide to switching DNS servers settings to tackle nameserver blocking & corporate proxy"
showFullContent = false
+++

### Motivation

Most of us, at some point of our lives, have used at least a custom DNS servers configurations for our computers.
Be it for avoiding censorship, faster browsing, security, or bypassing content restriction, we've all tried setting our DNS servers to some public DNS servers like one from Google (remember 8.8.8.8 and 8.8.4.4?).
I myself is also no exception as I have been using [1.1.1.1](https://1.1.1.1/dns/) for privacy and speed.
Unfortunately, having custom DNS nameservers also comes with some setbacks for someone who uses their computer all the time in all the different places.
With custom DNS servers, this is what you get on a network that tries to block them (together with VPN services etc.)

![blocked by network](/posts/dns-command/img/blocked.png)

Another problem that we would have with custom DNS servers is that networks' login pages just don't show up as the default DNS is not being used to redirect us to local network login page.
Of course it would be easy to just remove the configurations as you (most likely) was the one who set them up, how hard would it be to remove?
But what if we need these alternate DNS servers again?
I personally find this way of manually removing and adding them back too much of a hassle.
Hence, I wrapped them in a bash function that does the job for me as all lazy programmers do.

### 1.1.1.1

Before starting with explaining what I did in the next section, I would like to do a big shout-out to this awesome project that's a combined effort from [Cloudflare](https://www.cloudflare.com/) and [APNIC](https://www.apnic.net/) - [1.1.1.1](https://1.1.1.1/dns/).
It's an effort to provide Internet users with a faster and more secure DNS (it is more than 2x faster than Google Public DNS 🚀🚀🚀).
But more than just that, it is a **privacy-first** DNS server.
Did you think only these companies like Google, Facebook trying to track all of your activities online (which can be blocked with the awesome [Privacy Badger](https://www.eff.org/privacybadger))?
Your ISP also eavedrops on whichever site you are visiting and sell these data to target you with ads, and they do it through, guess what, DNS resolvers!

1.1.1.1 protects you from all of that for a price of \$0 (yes, you read that right).
A **FREE**, fast, secure DNS server that respects your privacy - sounds too good to be true, doesn't it? But it's an effort towards building a better Internet.
You don't have to take my words for that, read more about it on [Cloudflare's](https://blog.cloudflare.com/announcing-1111/) and [APNIC's](https://labs.apnic.net/?p=1127) blogs.

### Switching DNS nameservers via bash

As I mentioned previously, it's too much of a hassle for me to go manually update my DNS configurations via Network Settings UI whenever I jump back and forth between school's and home's networks.
Naturally, I would try to figure out how to do it via my terminal.
That's when I found out about this awesome tool that comes in-built with macOS - `networksetup`.
With networksetup, changing my DNS config is as simple as

```bash
# Adding 1.1.1.1 to my nameservers
networksetup -setdnsservers Wi-Fi 1.1.1.1
# Clear all my alternate nameservers
networksetup -setdnsservers Wi-Fi empty
```

Now I can easily configure my DNS resolvers to be the same as the instructions for 1.1.1.1 by

```bash
# 4 nameservers - 2 IPv4 & 2 IPv6
networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
```

But this is quite lengthy to type as well, even though I would prefer this over Network Preferences UI any day. So I went an another level of laziness by wrapping it inside a bash function

```bash
function dns1111() {
    networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001
}
```

This seems to be pretty neat and does the job, but hold on a second. How about switching it back to empty?
A flag/argument to this bash function would do the job fine but I want to use the least effort as possible and save myself the mental burden of remembering whether my alternate DNS config is empty.
So I thought to myself: "Why not detect the current state and switch the DNS config based on that? Like a literal switch button - you **don't** have to tell what state you want it to be, just the opposite of what comes before".
With that in mind, I found this option for networksetup that would tell me the current state

```bash
networksetup -getdnsservers Wi-Fi
```

It would list all of my alternate DNS servers, or print out this string `There aren't any DNS Servers set on Wi-Fi` if there isn't any.
And now I can complete my function, with some cool emojis to signify whether the config has just been applied or removed

```bash
function dns1111() {
  if [[ $(networksetup -getdnsservers Wi-Fi) == "There aren't any DNS Servers set on Wi-Fi"* ]]; then
    networksetup -setdnsservers Wi-Fi 1.1.1.1 1.0.0.1 2606:4700:4700::1111 2606:4700:4700::1001 && echo 🚀 🚀 🚀
  else
    networksetup -setdnsservers Wi-Fi empty && echo 🚦 🚦 🚦
  fi
}
```

Now I can easily switch back and forth between 1.1.1.1 and default DNS configs with just a short command `dns1111`

### Bonus: Linux based machine

Since `networksetup` is not available not Linux based OSes, we would need to use a different package that helps handle DNS nameservers called `dnsmasq`.

Install it using your distro's package manager

```bash
# this is for Ubuntu
sudo apt-get install dnsmasq
```

Installing the package will come with a config file `/etc/dnsmasq.conf` for us to update the nameservers.
I have included my implementation below.
If you have a better approach, don't hesitate to reach out to me at [hung.ngn.the@gmail.com](mailto:hung.ngn.the@gmail.com)

```bash
function dns1111() {
  # detect existence of the backup file to see whether the config is in place
  if [[ -f /etc/dnsmasq.conf.bak ]]; then
    sudo mv /etc/dnsmasq.conf.bak /etc/dnsmasq.conf && echo 🚦 🚦 🚦
  else
    sudo cp /etc/dnsmasq.conf /etc/dnsmasq.conf.bak && \
    sudo echo -e "server=1.1.1.1\nserver=1.0.0.1\nserver=2606:4700:4700::1111\nserver=2606:4700:4700::1001" >> /etc/dnsmasq.conf && \
    sudo systemctl restart dnsmasq  && \
    sudo systemctl restart NetworkManager  && \
    echo 🚀 🚀 🚀
  fi
}
```

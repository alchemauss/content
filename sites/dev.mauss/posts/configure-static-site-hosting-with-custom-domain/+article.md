---
date: "2020-06-19T17:38:25+07:00"
title: Configure Static Site Hosting with your Custom Domain
description: Quickly setup your custom domain in just a couple of steps
tags: [tutorial, tech, nameservers]
thumbnail: https://cdn.pixabay.com/photo/2019/01/31/20/52/web-3967926_960_720.jpg
---

You've just bought a custom domain to use for your personal website, specifically a static site using some static site hosting provider like Netlify, Vercel (formerly ZEIT Now), GitHub Pages, or other selected providers. But, you can't seem to connect the pieces together, or you've done it but the website in your domain doesn't seem as responsive or quick as if you didn't use your domain. We'll be discussing on how to solve those problems here.

## Set up custom domain with selected providers

### Netlify

Configuring with Netlify is pretty straightforward with their Web UI. We'll need to go to our sites and choose our already deployed site, go to its settings, and click on the `Domain management` on the sidebar, or use the following url and replace the `SITE_NAME` with your deployed site name.

```
https://app.netlify.com/sites/SITE_NAME/settings/domain
```

From there, it's simply clicking the `Add domain alias` button in the `Custom domains` section.

1. Enter your domain alias in the provided input. You could also use it with a subdomain like `sub.yourDomain.com` instead of just `yourDomain.com`.
2. It will then give you a warning to `Check DNS configuration`, click on it and it will pop up a DNS configuration section. You'll notice there's a **Recommended** option and **Alternative** option, we'll get to the Recommended option later on.
3. Copy the IP address on the Alternative option
4. Go to your domain registrar where you bought your domain and add the IP address you copied previously into the DNS records with the name of `@` if you're not using any subdomain and type `A`. If you use a subdomain, just replace `@` with your subdomain.

### Vercel

Configuring with Vercel is also pretty straightforward with the same approach as Netlify. We need to go to our deployed site settings first, click on `Domains` on the sidebar, and enter your custom domain in the provided input and click the add button. Another way is to use the following url and replace the `USERNAME` and `SITE_NAME` with your username and deployed site name respectively.

```
https://vercel.com/USERNAME/SITE_NAME/settings/domains
```

From there, it will show an `Invalid Configuration` at first, click on the `ANAME` tab and copy the value. The steps are similar to our previous section where you'll need to go to your domain registrar and paste the copied value to the DNS records.

### GitHub Pages

Using GitHub Pages is simply having a repository with the name exactly like the given format and replace `USERNAME` with your GitHub username.

```
USERNAME.github.io
```

Have your domain registrar point a DNS record with the value of `USERNAME.github.io`, and then create a `CNAME` with a single line inside with the domain you want it to be, add and commit the file and push it to the repository.

## Getting the most out of their services

You'll notice that Netlify has a Recommended option that we didn't follow in the previous section, and Vercel also gives us a warning for using the `ANAME` tab option. They even said it in the warning that

> This will require a DNS provider with support for ALIAS or ANAME records and will likely turn off our CDN and reduce performance.

It turns out it's true with these static site hosting providers, because they operate using a CDN to serve our site, there's quite a significant performance loss if you're not directly using their CDN. I discovered this while I was setting up this website with my custom domain and they're not performing well in terms of initial load, it messes up with the scripts loading so much that it prevents users from navigating through the site if they're not given enough time to be fetched at first load.

To prevent that from happening, we'll need to reroute the whole domain to our selected provider, be it Netlify, Vercel, or others similar by adding their name servers as the custom one in our domain registrar settings.

Remember from our previous steps where they gave us their name servers, another easier way is to immediately add your custom domain to their domain management tabs. Use one of the following url for your selected provider.

```
#$ file: Netlify Domain Management
https://app.netlify.com/teams/TEAM_NAME/dns
```

```
#$ file: Vercel Domain Management
https://vercel.com/dashboard/domains
```

After adding our custom domain, we could simply go to their settings and get the name servers. Netlify usually have 4 to be assigned, and Vercel has 2. It might be different for some of you, but they usually have numbers in it.

```
#$ file: Netlify Nameservers
dns1.p05.nsone.net
dns2.p05.nsone.net
dns3.p05.nsone.net
dns4.p05.nsone.net
```

```
#$ file: Vercel Nameservers
ns1.vercel-dns.com
ns2.vercel-dns.com
```

They say it might take from 24 hours up to 48 hours to take effect, but it's usually a lot faster than that, probably around 1 hour give or take. After that, you can freely use your custom domain to point your domain or any subdomains to any site or IP address, just like you could previously when it's still with your domain registrar nameservers. But, now you'll have full performance from their CDN and other services.

Some of the only downsides I find with using their domain management service is that you cannot use your domain registrar features anymore. You can't have the inbuilt DNSSEC or email forwarding from Google for example. You're basically limited to what the provider has to offer for you and they're usually not as feature-rich as a specifically built for DNS management provider like Cloudflare.

---
Reference(s):

- <https://docs.netlify.com/domains-https/custom-domains/#dns-configuration>
- <https://vercel.com/docs/v2/custom-domains>
- <https://pages.github.com/>

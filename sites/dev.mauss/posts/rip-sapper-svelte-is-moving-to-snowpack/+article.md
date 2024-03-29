---
date: "2020-10-20T17:38:25+07:00"
title: RIP Sapper - Svelte Is Moving to Snowpack
description: It's been a fun ride using Sapper but all good things must come to an end. Svelte is moving to Snowpack and ditching bundlers
tags: [tech, svelte, sapper, snowpack]
---

Sapper is the official framework for Svelte to build full-fledged applications with the ability to do SSR, be a SSG, and have SPA functionality all together. It has been on early development stage for some time now and finally, on Sunday night (or Monday morning my time) 18 October 2018 at Svelte Summit 2020, Rich Harris announced that Sapper 1.0 will **never come**.

> The codebase is kinda gnarly and it's hard to work with, it's tricky to add valuable features like building for serverless platforms, or pre-rendering a subset of your application as static files

He explained that the future of web development would be moving to a better more future proof alternative, hence they won't be working on Sapper anymore on the long run and instead focus on Svelte itself.

![!YouTube#hb](qSfdtmcZ4d0 "Rich Harris: Futuristic Web Development")

Svelte would have SSR built-in and will rely more on Hot-Module Reload (HMR) by using Snowpack, rather than the conventional way of bundling everything up, which slows down development a lot. In return, the team have started working on `@sveltejs/kit` as a replacement for Sapper with a bundler and have ported most of the good stuff from Sapper to this new kit. This would be the standard, one-for-all answer to the long and ongoing question on "How do I get started, do I use Svelte or Sapper", Svelte Kit (name may change in the future) will be answer and official solution to settle this once and for all.

Of course, it's by no means ready for prime time, the team still has a lot of plans to improve before they release it to the public. Yes, it's still in private development, so it's not open to public contributions, yet. In the mean time, I don't expect Sapper to immediately "die" and stop all future developments, I'm certain there will be someone who will still work on it and continue its legacy, keep improving and maintaining its codebase, it is open-source after all.

![svelte chooses kit over sapper](https://cdn.discordapp.com/attachments/728292755087818924/767513340384903178/4iz1gv.png "[[Discord](https://discord.com/channels/457912077277855764/728292755087818924/767513340615458866) | [Preview](https://cdn.discordapp.com/attachments/728292755087818924/767513340384903178/4iz1gv.png)] Meme by swyx from Discord chat")

Personally, I'm really sad to see that HMR isn't coming to Sapper because I first started with Sapper and learned Svelte by building Sapper apps. So, you could also guess that I've made most of my apps with Sapper. I guess all good things must come to an end, and I'm sure that this would be a way better solution in the future, but I'll probably need some time to take this in.

Don't take the title literally, that is how most people view it. It doesn't mean that you can't write Sapper apps anymore, or you should immediately abandon your current projects too. Right now Sapper is way more mature and ready for production, there's a lot of people who can testify this. My recommendation is to stick with Sapper right now and wait until Svelte Kit (name may change in the future) is actually stable and ready for use. Since it has a lot of similarity on its convention and API as Sapper, migrating later won't be a problem at all.

> I think we are going to get to a point pretty soon where projects that use bundlers during development are going to look a little bit antiquated

So far, the only downside I could see from using Sapper is that your development experience and time it takes to bundle will degrade as your application gets bigger and more complex, even simple changes will take at least 10 seconds to rebuild. Other than that, it's perfectly fine and I can comfortably say that until Svelte Kit (name may change in the future) is out, Sapper is the best choice for you to start working on your Svelte app.

***
Reference(s):

- <https://www.reddit.com/r/sveltejs/comments/jdojpd/sapper_is_dead_long_live_svelte>

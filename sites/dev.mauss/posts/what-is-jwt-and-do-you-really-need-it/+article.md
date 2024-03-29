---
date: "2020-09-28T17:38:25+07:00"
title: What Is JWT and Do You Really Need It?
description: What is a JSON Web Token and do you really need it for your app.
tags: [jwt, javascript, session]
---

JSON Web Token (JWT) is technically just a secure wrapper for some JSON data. It's an encoded string that's URL safe, can contain unlimited data unlike cookie, and cryptographically signed. It's great because no middleman can modify a JWT once it's sent and the server can always guarantee the data by checking the signature. Hence, it's so popular that [sessions can become stateless now](https://auth0.com/blog/stateless-auth-for-stateful-minds/).

## TL;DR: Should you use JWT

As stated above, JWT is great for API authentication, server-to-server authorization, and as single-use authorization token. But, it's not a good choice and shouldn't be used for sessions. **Doesn't mean you can't use it**, read on to understand the benefits and drawbacks to decide for your own.

## Uses

What is JWT good for, where and when do you need to use them, how do you actually use them, why do we need it when we have cookies and bearer tokens?

A lot of people have a mistake of trying to compare "cookies vs. JWT", this makes no sense as cookies are *storage mechanism*, whereas JWT tokens are *cryptographically signed tokens*. It's like comparing apples to oranges. They aren't opposites, instead they can be used independently or together. The correct comparison should be "sessions vs. JWT" and "cookies vs. localStorage".

It's also completely reasonable to combine both sessions and JWT, again read on to understand and decide for yourself when to appropriately use each one.

Citation: <http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/>

### Cookie and Bearer Token

Let's make sure that we're talking about the same thing first, both cookies and bearer tokens send data. So, how does cookies and bearer tokens play a part in JWTs?

A cookie is a name-value pair stored in the browser with an expiry date and associated domain. We can store cookies with either JavaScript or an HTTP Response header. Cookies set with Response headers usually have their `httpOnly` set to true, this means it cannot be read with client-side JavaScript, usually to prevent XSS attacks.

```javascript
document.cookie = 'cookie_key=value'   // JavaScript
Set-Cookie: cookie_key=value           // HTTP Response Header
```

All browser will automatically attach cookies with every request to the cookie's domain, we can also explicitly set it to include cookies to other domain by specifying the `credentials` options in our `fetch` requests.

On the other hand, bearer token is a value that goes into the `Authorization` header of your requests. It's not automatically stored like cookies, has no expiry data, and no associated domain. It's simply just a value that we can choose to store and include with our requests.

```javascript
// Example with fetch API from client-side JS
fetch('https://your.domain', {
  headers: {
    Authorization: `Bearer ${bearer_token_value}`
  }
})
```

### JWT and Token Based Auth

Usually a token received from a trusted authority or third-party provider, we can use it to send or receive data with the provider. We'll want to store the token somewhere in the browser so our user can reuse it.

Our first option is to store it in a cookie and let the server send cookie headers for each request, another option is to store it in a local or session storage and manually set the `Authorization` header for each request sent. Both will function the same and the server will receive and proceeds to parse it nonetheless.

Ref: <https://stackoverflow.com/questions/37582444/jwt-vs-cookies-for-token-based-authentication>

### Cookie-based vs. Token-based Authentication

Cookies can set their `httpOnly` and `Secure` flag on to prevent XSS and transmission over unencrypted channel, but third-party cookies are still vulnerable to CSRF attacks since it's sent by default to their respective domains. It also suffers in performance and scalability because of their stateful nature, and as such the server needs to store them in a file or database to maintain the state of all the users. The backend server also needs to maintain a separate system to store all the session cookies as the user base increases.

Meanwhile, tokens excels in their performance and their ability to scale, as well as mitigate CSRF attacks as they're not sent to third-party domains by default. Their performance are great because they are self-contained with all the headers, metadata, and signature needed for *stateless sessions*. Of course, they're also scalable by default since everything is self-contained and there's no need for the server to maintain any user sessions. But, they are susceptible to XSS attacks since they're stored in local storage and accessible to client-side JavaScript of the same domain.

Ref: <https://security.stackexchange.com/questions/180357/store-auth-token-in-cookie-or-header>

## Securing JWTs

We've discussed how we can use JWT with either cookie or as bearer tokens, and we've also look on the difference for authentication between using a cookie and token. How do we truly prevent, or at least mitigate and secure our app as much as possible?

Now, JWT claims it prevents CSRF attacks, it doesn't really. Using JWT doesn't have anything to do with preventing attacks like this, it's how we store it. As discussed before, we can store JWT in a cookie or elsewhere like local storage. Storing it in a cookie makes it vulnerable to CSRF attacks, but storing it in the browser makes it vulnerable to other different, potentially worse class of vulnerabilities like XSS and such.

To mitigate these vulnerabilities, we should secure the whole app in general. A simple solution is to store our JWT in a cookie with the `httpOnly` flag set to true.

## Drawbacks

It seems too good to be true, what's the catch then, there must be a drawback to all of this? Yes. It's important to know that JWT is only serialized and not encrypted, it guarantees data ownership but not encryption, so the data can still be seen by anyone that intercepts the token. This is simply solved by using HTTPS, and not just for JWT but you should use HTTPS in general too.

![Sarcastic JWT Flowchart](http://cryto.net/%7Ejoepie91/blog/attachments/jwt-flowchart.png "[Why JWT won't work flowchart](http://cryto.net/~joepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/) by Sven Slootweg (joepie91)")

### Size

This is the biggest disadvantage we'll encounter when using JWT. Below is the difference in size between 2 identical data storing a user ID (abc123), one in a cookie and the other in JWT. Our data stored in a cookie have a total size of 6 bytes, compare that to 304 bytes when storing in a JWT, that's around 51x the size of the original data.

![Regular vs JWT](https://scotch-res.cloudinary.com/image/upload/dpr_1,w_800,q_auto:good,f_auto/media/36632/3BubRdY1SFyt8d6Rf2OP_jwt-size.png "[Why JWTs Suck as Session Tokens](https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens)")

304 bytes might not seem that big of a deal, but let's say your site gets roughly 100k views per month, using JWTs you'll be consuming an additional ~24MB of bandwidth each month. All the little things start to add up, and keep in mind that we're only storing a user ID inside the JWT, it's not even the bare minimum.

### Serialized not encrypted

Another drawback which we've mentioned previously is that JWT is still just a serialized data passed on between servers. It's not encrypted and anyone that can get their hands on this can read what's inside. There's another standard called [JSON Web Encryption (JWE)](https://tools.ietf.org/html/rfc7516) which offers encryption to the JSON data.

### CRUD to the database anyway

**Most websites** that require user login are primarily generating dynamic user content, so the user object needs to be loaded from cache or database anyway. The stateless benefits of JWT are not being taken advantage of, which is the main selling point of using JWT in the first place.

Almost every framework loads the user on every incoming request, so even for sites that are primary stateless, the application is still loading the user object regardless. To compound this issue, JWT are larger because of their cryptographic signatures, hence they need more time and CPU power to compute compared to traditional sessions.

Ref: <https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens#toc-why-do-jwts-suck->

***
Reference(s):

- <https://blog.logrocket.com/jwt-authentication-best-practices/>
- <https://scotch.io/bar-talk/why-jwts-suck-as-session-tokens>
- <http://cryto.net/~joepie91/blog/2016/06/13/stop-using-jwt-for-sessions/>
- <http://cryto.net/%7Ejoepie91/blog/2016/06/19/stop-using-jwt-for-sessions-part-2-why-your-solution-doesnt-work/>
- <http://self-issued.info/docs/draft-ietf-jose-json-web-encryption.html>
- <https://stackoverflow.com/questions/37582444/jwt-vs-cookies-for-token-based-authentication>
- <https://security.stackexchange.com/questions/180357/store-auth-token-in-cookie-or-header>

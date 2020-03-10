# XSS prevention: sandboxing
## iframes
As some of the other defense measures seen before, sandboxing is not only a prevention against cross-site scripting. However, when used it is very effective against XSS attacks as well, since one feature of a sandbox is that scripts may not be run at all. 

Let's immediately look at an example of a sandbox, so its use becomes clear. You may be using an iframe, like so:

```
<!DOCTYPE html>
<html>
<body>

<iframe width="400" height="500" frameborder="0" src="https://www.bbc.com/news/av/embed/p0866knz/51820740"></iframe>

</body>
</html> 
```

The above HTML page would embed the BBC video. BBC could decide to run JavaScript in that iframe, but that would not harm you because the iframe is from a different _origin_ then the top level page. We've covered the same-origin policy before, and it applies just as well to iframes. However, the iframe could do some other unpleasant actions. 
* it could show a form to the user that looks like a password form coming from your website;
* it could start annoying you with a lot of pop-ups
* it could try locking your pointer (see https://mdn.github.io/dom-examples/pointer-lock/ for a demo)
* it could automatically start playing a video
* ...

Moreover, IF the iframe is hosted on the same origin as the top level page, any JavaScript running inside it could have an impact on the complete document. Then again, if the iframe is loaded from a trusted origin, you most likely trust it. But still, in some cases you may want to embed a same-origin iframe, but tell the browser to give it a random origin so it could certainly not mess with the origin of the top level document. **The most important property of a sandbox (if you do not give it the `allow-same-origin permission`), is that the browser will assign it a random origin.**

If we sandbox the iframe of the above example, the page becomes the following: 

```
<!DOCTYPE html>
<html>
<body>

<iframe width="400" height="500" frameborder="0" src="https://www.bbc.com/news/av/embed/p0866knz/51820740" sandbox="allow-scripts allow-same-origin" ></iframe>

</body>
</html>
```

The permissions `allow-scripts` and `allow-same-origin` are given, since otherwise the video does not load. That is, we must allow the BBC iframe to run any script it wants in the origin of `https://www.bbc.com`. The reason it requires these permissions is most likely that this frame loads its contents from a set of APIs. 

**NEVER** use `allow-scripts` and `allow-same-origin` together if your iframe has the same origin as the top level document. In that case, the frame will have the same origin as the top level document, and since it has permission to run script it can simply remove the sandbox attribute from its own iframe element. 

## CSP
A sandbox directive exists for CSP as well, allowing to sandbox the requested resource. This allows you to restrict the usage of pp-ups, plugins, or scripts and assign the requested resource a random origin. 

Let's imagine that you build a website, where there is one section for admins (`https://www.example.org/admin/...`), and another section for users (`https://www.example.org/users/...`). Each user is given a profile page, and users have complete control over their page. There are no input nor output validations on their profile pages. Now, sometimes the administrators of that website will access a user's profile page. Since the user section and the admin section are running on the same origin (only the path differs), this is a security nightmare. However, what if you configure your web server to always return the following CSP header for any page in the path `users/...` :

```
Content-Security-Policy: sandbox allow-forms allow-modals allow-popups allow-scripts;
```

You would allow a whole lot, but you would not allow `allow-same-origin`. Therefore, the browser would consider any page in the path `users/...` as a random origin, limiting the attack surface significantly. 
---
title: "Angular/Asp.Net Core Server Side Rendering for Authenticated Users"
header:
description: How to get server side rendering work for authenticated requestes using cookies.
og_image: https://i.imgur.com/HSRXWaI.png
---

This is a follow up to “[Angular/Asp.Net Core Authentication with Identity Server 4.](https://medium.com/@lankapura/angular-asp-net-core-authentication-with-identity-server-4-65db6b9f7c2a)” I’ll be using the same source code in this post.

Once you set up authentication in the Angular application, you will notice that the server side renderer no longer returns pre-rendered HTML.

![Imgur](https://i.imgur.com/DkbH2ix.gif)

This is because angular-oauth2-oidc is using browser session storage to store the OAuth token that received from the Identity Server. Therefore, the server pre-renderer can’t make authenticated HTTP requests, since session storage is not available during the server pre-rendering step. So how are we going to send the OAuth token to the server side?

![Imgur](https://i.imgur.com/F3WDhAt.png)

By using good old **Cookies**!

angular-oauth2-oidc has a OAuthStorage interface that we can use to implement and define our own token storage implementation. But we need different implementations for client side and server side. This is because, even though we can access cookies directly on the client side, it is not possible to do so during server pre-rendering due to the nature of how pre-rendering works inside ASP.NET core SPA services. Could there be a way to access cookies during pre-rendering?

<script src="https://gist.github.com/lankaapura/c429e293f2fc2a1415c498cde251743a.js"></script>


ASP.NET core has provided the ‘asp-prerender-data’ prop to pass any data to the server renderer function. Using that, we can retrieve the same parameter and pass it to Angular as a provider which can be resolved in any Angular service or component.


<script src="https://gist.github.com/lankaapura/0e14d5ff7abbb8053e0dcb30fd0abe18.js"></script>

The next step is to implement the Token store service for server renderer. Here, we can resolve ‘cookies’ which contains cookies of the current request. So when the server renderer requests the OAuth token, it can be retrieved from cookies.

<script src="https://gist.github.com/lankaapura/ff28602b18a1fd86387bf03ae9d10f87.js"></script>

Finally, we need to tell Angular to use our token store implementations for OAuthStorage interface.


<script src="https://gist.github.com/lankaapura/2fdfe99ffae4265e30bb89d0253e1e4b.js"></script>

Once the user is authenticated from Identity server, the OAuth token should be available to the server renderer now.

![Imgur](https://i.imgur.com/lA2SYN0.png)

Therefore, we should be able to see pre-rendered HTML during the initial loading of our application.

![Imgur](https://i.imgur.com/PCTYMst.gif)

Complete source code can be found on [GitHub](https://github.com/lankaapura/Angular-AspNetCore-Idsvr/).

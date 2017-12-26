---
title: "Angular Asp Net Core Authentication With Identity Server 4"
header:
  og_image: https://i.imgur.com/HSRXWaI.png
---

Setting up Authentication for applications can be intimidating. Usually it doesn’t involve writing a lot of code, because [good][1] [folks][2] [like][3] [these][4] [have][5] [invested][6] [their][7] time to do all the hard work for us. As a result, most of the time we are dealing with configurations. Setting up authentication for Angular SPA is no different.

![Imgur](https://i.imgur.com/AkiMEXc.png)

This post covers the following topics:

1. Integrating Angular SPA with Identity Server Implicit Flow
2. Configuring ASP.NET core web API to validate tokens

This post doesn’t cover setting up Identity Server. I’ll be using this [Identity Server demo site](https://demo.identityserver.io/) in my sample since it can be easily accessed and integrated. Read [this post](https://www.scottbrady91.com/Identity-Server/Getting-Started-with-IdentityServer-4) by [Scott Brady](https://twitter.com/scottbrady91) to learn more about Identity Server.


## Identity Server Implicit Flow

>The implicit grant type is optimized for browser-based applications. Either for user authentication-only (both server-side and JavaScript applications), or authentication and access token requests (JavaScript applications).
>
>http://docs.identityserver.io

## Create Angular Project
Let’s create an angular application using dotnet CLI.

```
dotnet new angular
```
Default template has old angular packages. So, let’s update them to their latest versions. We will be using Angular 4.4.6 Since the [AspNet core template doesn't support Angular 5 properly](https://github.com/aspnet/JavaScriptServices/issues/1288).

![Imgur](https://i.imgur.com/4f89ilv.png)

## Angular OAuth Packages
Identity Server has provided a JavaScript plugin [odic-client-js](https://github.com/IdentityModel/oidc-client-js) to integrate browser based applications. However, it was not implemented targeting modern browser-based applications such as Angular and React. So you will run in to issues when using server-side rendering and AoT compilation. Thankfully, the community has come forward with solutions! There are two [OpendID Certified](https://openid.net/certification/) plugins for Angular:

* [angular-oauth2-oidc](https://github.com/manfredsteyer/angular-oauth2-oidc) by Manfred Steyer
* [angular-auth-oidc-client](https://github.com/manfredsteyer/angular-oauth2-oidc) by Damien Bowden

I chose angular-oauth2-oidc since it supports password flow as well.

```
npm i angular-oauth2-oidc --save
```
I had to install es-promise package to prevent build failure.

```
npm install es6-promise --save
```

## Angular-oauth2-oidc Setup
Now that we have the base project, it’s time to configure angular SPA for identity server. First, we should import the OAuth Module and HttpClient Module to our application.

<script src="https://gist.github.com/lankaapura/9a7d8f04fdde8cf25513b4a875442d5a.js"></script>

Then we should define Identity sever details.

<script src="https://gist.github.com/lankaapura/38ee686271ceb81c36639729adb6e449.js"></script>

Next, OAuth Module should be initialized using config object. We don’t need our application trying to connect to Identity Server and retrieve tokens during server pre-rendering, so everything is wrapped inside isPlatformBrowser code blocks.

<script src="https://gist.github.com/lankaapura/4d254f38c7345ad03ea427e4a513cefd.js"></script>

Now, how are we going to force users to log-in to our application?

## Setup Route Guard
We can define a logic in a route guard to be validated before navigating to a requested route. Using a route guard, we can prevent users navigating to our application without a valid token.

<script src="https://gist.github.com/lankaapura/c38bda701bcf01d44e056dab519f1ae2.js"></script>

## Setup OAuth Callback
If you check the authConfig redirect URI, Identity server is configured to redirect to ‘/’ after a successful authentication attempt. It is important **NOT** to have an AuthGuard for that particular route. Otherwise the application won’t be able to retrieve the token from the query string, resulting in a redirect loop.

<script src="https://gist.github.com/lankaapura/3a9e0d758d3d876d120f98b40e6e3902.js"></script>

Now we should be getting a token from Identity server. But how do we add it to our API requests and validate it from Web API side?

## Making HTTP Requests with OAuth Tokens
We can use HTTP interceptor, that was introduced in Angular 4.3, to include the OAuth token in all HTTP requests. You might notice it resolves dependencies differently. The [reason is a known issue](https://github.com/angular/angular/issues/18224f) which makes circular dependencies between modules.

<script src="https://gist.github.com/lankaapura/240dcc08b4ec45d5e2b8789e3f693630.js"></script>

## ASP.NET Core OAuth Token Validator
Finally we need to validate OAuth tokens in the web server. That can be easily achieved using the Identity Server Access Token Validation package.

```
dotnet add package IdentityServer4.AccessTokenValidation
```

<script src="https://gist.github.com/lankaapura/f7b13feb33e8958f77479b89961cfe04.js"></script>

Now Angular SPA should be redirected to Identity server to authenticate, and should redirect back to angular SPA upon successful authentication.

![Imgur](https://i.imgur.com/pCLiQkE.gif)

Full source code can be found on [GitHub](https://github.com/lankaapura/Angular-AspNetCore-Idsvr/tree/edf1c542e4821b3b0a8976412afca7b917634a42).

Next, how do we use server side rendering for Authenticated users? Find out in my [next post](https://medium.com/lankapura/angular-server-side-rendering-for-authenticated-users-a021627fd9d3).

[1]:https://github.com/IdentityServer/IdentityServer4/graphs/contributors
[2]:https://github.com/openiddict/openiddict-core/graphs/contributors
[3]:https://github.com/aspnet/Identity/graphs/contributors 
[4]:https://github.com/keycloak/keycloak/graphs/contributors
[5]:https://github.com/manfredsteyer/angular-oauth2-oidc/graphs/contributors
[6]:https://github.com/jaredhanson/passport/graphs/contributors
[7]:https://github.com/oauthjs/node-oauth2-server/graphs/contributors
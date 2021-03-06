---
title: 'ngrok'
tags: ['ngrok', 'network']
published: true
date: '2018-12-08'
---
Another subject I know very little about is networking. I know my computer has to connect to wifi to access the internet. But if you held a gun to my head and asked me to explain _how_ that works, this would be a very short post.

I do know that when setting up tests, running services locally is usually the path of least resistance. If you're running tests from the same machine, there's generally no friction.

But sometimes you need to share a locally running resource with a different machine that may or may not be on the same network. You can go down the rabbit hole of trying to deploy to a publicly available site, or go the easy route and set up a tunnel to your local machine.

Enter [ngrok](https://ngrok.com/). This is a service that allows you to open up a port on your local machine and make it publicly available.

To install: ```npm install -g ngrok```

You need to create a free account with ngrok, but they support both Github and Google auth so you can be up and running in just a few minutes. After logging in, running ngrok with a protocol and a port number will give you a publicly facing URL that is valid for 24 hours. (I'm pretty sure they have paid versions with more flexibility, but I haven't need that yet.)

### TL;DR
```ngrok http 8081```

```ngrok help```
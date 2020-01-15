---
title: 'More tunnels'
tags: ['serveo', 'localtunnel', 'ngrok', 'network']
published: true
date: '2019-03-01'
---

I've been using [ngrok](/ngrok/) for a while and have become a big fan of it. But one of the clients I work
with is a large company with a large IT department and I assume a large committee that sits around
a large table discussing what apps they should block and dreaming up new firewall rules.

So ngrok doesn't always work where I need it to. So I've had to rely on a few alternates, namely [serveo](http://serveo.net/)
and [localtunnel](https://github.com/localtunnel/localtunnel).

### localtunnel
```npm install -g localtunnel```

```lt --port 3000```

### serveo
```ssh -o ServerAliveInterval=60 -R 80:localhost:3000 serveo.net```

### TL;DR
* localtunnel will work in a pinch, but I noticed some slowness when using it
* serveo doesn't require any installation and tries to re-use the same URL when it reconnects

### Update
[localhost.run](http://localhost.run/) is also an option.

```ssh -R 80:localhost:8080 ssh.localhost.run```
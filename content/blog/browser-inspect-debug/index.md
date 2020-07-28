---
title: 'Browser dev tools debugging'
tags: ['web']
published: true
date: '2020-07-26'
---

I find myself lately in the unfortunate position of doing a lot of web-based UI
automation. Which means I spend a lot of time in the inspector tab of my browser's
developer tools. I only recently learned you can send a debug command to stop the
DOM from changing while you're trying to inspect it. I wish I had just asked
about this years ago.

```
setTimeout(function(){debugger;}, 5000)
```

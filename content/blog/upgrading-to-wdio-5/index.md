---
title: Updating to WebdriverIO 5, Part 1
author_name: Marc
tags:
  - webdriverio
  - selenium
  - yarn
  - npm
  - typescript
---

I generally have a bad habit of putting off software updates as long as possible. My Macbook
has been nagging me about OS updates daily for about the past 3 months and I keep clicking 'Remind
Me Tomorrow' each time.

But I've got a couple days off work and [WebdriverIO](https://webdriver.io/blog/2018/12/19/webdriverio-v5-released.html)
just released v5 so I thought I'd take a chance and see what it would take to update. My initial thought was that
it would be a couple hours of quick updates or a couple hours to determine it's going to be more effort than it's
worth.

So I started with their step-by-step instructions:

### read the [changelog](https://github.com/webdriverio/webdriverio/blob/master/CHANGELOG.md#v500-2018-12-20) to understand all breaking changes
Reading through the changelog made me optimistic that I was going to have to do a few page object updates
and be done.

### remove all wdio-* packages from your package.json
Easy enough. A handful of ```yarn remove``` commands and moving forward.

### remove your node_modules director
Easy peasy.

### install the latest version of webdriverio: $ npm install webdriverio@latest
Piece of cake. Just sub ```yarn add``` for ```npm install```. It's worth noting at this point that
I made several technical decisions early on in this automation project based on what my development team
is using. Personally, I don't have strong opinions between npm and Yarn. But my dev team is using Yarn
so I went with that to keep dev and testing as aligned as possible.

### install the new wdio testrunner: $ npm install @wdio/cli --save-dev
Sure thing.

### if you have a wdio.conf.js in your root directory, create a backup: $ cp wdio.conf.js wdio_backup.conf.js
I actually backed up the file. Not just nuke it and rely on GitHub having my back. Actually created a backup copy.

### rerun the configuration wizard: $ ./node_modules/.bin/wdio config
The last four steps went swimmingly easy and I'm starting to feel confident at this point. But this is where things
start to go pear shaped for me. Running the config script is giving me errors that it can't find the template to 
create the config file. A quick search and it looks like I'm up against [this issue](https://github.com/webdriverio/webdriverio/issues/2215).

I wasn't in the mood to deal with esoteric Yarn issues; as I mentioned earlier, I don't have strong opinions about
Yarn vs. npm. The path of least resistance was ```rm -rf node_modules && npm install```. I figured I could re-visit
Yarn later. Seeing tests run was more important for today.

### merge custom modifications of your old wdio_backup.conf.js into your new config file. Don't merge everything at once - just begin with the basic setup using no services and just the e.g. spec reporter to run tests locally and work torwards a full migration
I was actually looking forward to this. My current config file is a nightmare of months of trying out various configs
and then commenting them out instead of deleting them. So starting from a clean config felt good.

The only minor issue I ran across here is that I had forgotten that selenium-standalone is not part of the WebdriverIO
project so I needed to install it.

### take the simplest test in your suite and rename the commands according to the changelog
This is where everything went out the window. My simplest test was failing and not giving any meaningful error
messages. About 20 minutes in to debugging I realize I don't have the correct types in my project.

Did I forget to mention I'm using TypeScript for this project? 

Quick search and it looks like [typing is in progress](https://github.com/webdriverio/webdriverio/pull/2862) and was
de-scoped from the v5 initial release.

### TL;DR
1. Yarn is currently causing problems with generating a config file, but I'm pretty sure I can work around that.
2. I'm putting this on hold until types are available. Hopefully soon.
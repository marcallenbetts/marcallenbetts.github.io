---
title: 'Android network security (aka "How to monitor https traffic from Android")'
tags: ['android']
published: true
date: '2020-11-19'
---

Starting at around version 7 or 8 (I can't recall and am too lazy to look it
up right now), the Android OS started putting restrictions on the ability to
monitor https requests from apps. Luckily, it's fairly easy to create debug
builds that allow this.

In `app/src/debug/res/xml/network_security_config.xml`, add this:

```
<network-security-config>
   <base-config>
      <trust-anchors>
          <certificates src="system" />
          <certificates src="user" />
      </trust-anchors>
   </base-config>
</network-security-config>
```

And in `app/src/debug/AndroidManifest.xml`, add this:

```
<application android:networkSecurityConfig="@xml/network_security_config" />
```

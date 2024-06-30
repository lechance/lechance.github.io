---
title: what is minifyEnabled
date: 2015-05-03 21:05:15
categories:
- Android
tags:
- android
- proGuard
- minifyEnabled
---
minify is an Android tool that will decrease the size of your application when you go to build it. It's extremely useful as it means smaller apk files! It detects any code or libraries that aren't being used and ignores them from your final apk.

The only "disadvantage" to using minify is that if a user experiences a crash and reports it to you, the code is obfuscated and just looks like jumble! I put disadvantage in quotes there because it does actually assist in preventing people from reverse engineering your app to find exploits/passwords/API keys. See [this section of the ProGuard documentation][proguard-decoding] for details on how to decode a stack trace (ProGuard got renamed to minify but it is still called ProGuard in the documentation).

One important note though! If you're using minify, be sure to save your decoding key for EVERY release. You can't expect every one of your users to be running the latest version of your app so if they get a crash, you want to be able to read all of the crash logs, for every version.

Your decode key is stored under the build/outs directory in a mapping.txt file. Be sure to save it in a safe place! Also, note that the mapping.txt file will be overwritten for every app release so give it a new name and I'd also recommend you store it elsewhere than the build/outs directory in case it gets wiped for some reason (Hopefully not but, best to be on the safe side!)

[proguard-decoding]: http://developer.android.com/tools/help/proguard.html#decoding

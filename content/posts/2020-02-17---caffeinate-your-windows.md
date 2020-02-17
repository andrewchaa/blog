---
title: Caffeinate Your Windows
date: "2020-02-17T00:00:00.000Z"
template: "post"
category: "Powershell"
tags: 
  - "Utilities"
draft: false
slug: "/posts/caffeinate-your-windows/"
description: "ASP.NET Core has in-built dependency injection container and it’s pretty good enough to use. I use TestApiFactory class to use it without too much set up, but this time, I had to wire up service provider myself, as thses tests run against Service Fabric worker process which is an executable."
socialImage: "/media/42-line-bible.jpg"

---

Your laptop has screen-lock policy. Definitely, it's a good security practice that the screen locks if you don't do anything. 
Yet, always there are situations exceptional. You are doing a presentation and your screen locks while you are talking. 
You are in a meeting and you have to press keys constantly. It's distracting and annyoing.

So, you want to and need to caffeinate your machine.

Mac has [Amphetamine](https://apps.apple.com/us/app/amphetamine/id937984704?mt=12)
I know Windows has an app too, but I wrote a simple powershell script in case you can't install it. Often it's a case in corporate environment.

```powershell
param($minutes = 60)

Write-Host "Caffeinating the Screen for $minutes minutes"

$myshell = New-Object -com "Wscript.Shell"
for ($i = 0; $i -lt $minutes; $i++) {
  Start-Sleep -Seconds 60
  $myshell.sendkeys("{SCROLLLOCK}")
}

Write-Host "The caffeine is worn out now"

```


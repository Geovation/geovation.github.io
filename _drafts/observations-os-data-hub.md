---
title: "Some observations regarding the OS Data Hub"
meta: "My experience using the OS Data Hub has not been a good one.  I can't see how I can offer this service to our startups as it stands"
categories: Insights OS
author: "Paul Nebel"
slug: "observations-os-data-hub"
---

I have been working since January with one of the startups on the Registers of Scotland/Geovation Accelerator Programme.  That startup is called **Walks and Waterfalls** (henceforth referred to as **W&W**) and it is an app for helping outdoor enthusiasts in Scotland visit lesser-/un-know beauty spots featuring waterfalls in the Scottish countryside.

The founder of this startup is called Oscar.  From the very beginning Oscar has been extremely keen to us OS Maps as the background for his app as he is very familiar and happy with the look and feel of OS Explorer and Landranger maps.
 I have been building his application for him and this blog is a summary of what I have experienced when using the OS Maps API from the OS Data Hub (*spoiler alert - the experience has been terrible!*)
 
## First encounter with OS Maps API
I initially used the 'old' OS Maps API (https://api2.ordnancesurvey.co.uk/mapping_api/v1/service/wmts) to access the Leisure Raster maps to use as a background for W&W.

My first reaction was to ask myself why this service uses raster images and not vector images?  This seems like a strange decision given that these are digital products, and raster tiles are much larger and take much longer to download than vector tiles.

 > Why are we offering raster tiles via the OS Maps API?  Why aren't they all vector tiles which are much more performant?

I applied for and received an API key for this service.  As I recall, this was a fairly easy process.  I integrated the maps into W&W and for some time was very happy with the performance of the Maps API.  I then became aware of the new OS Data Hub and assocoated Maps API.

In particular I was very happy to learn of the API Dashboard that monitored free vs premium data usage which would be very useful when attempting to calculate how much Oscar would have to pay for the product.  This is an improvement on previous attempts at providing APIs commercially for the following reason: Startups (and, I would argue, any sensible business) cannot use a product or service for which they cannot calculate the cost.  This was an issue for the first month or so of usage, but that was because the Data Hub team were waiting for the PSGA to be agreed before they could confirm prices.  This was initially an issue but was resolved.

So, much like the 'old' Maps API I applied for an API key which I also recall as being a very easy process.  I updated the URL in the W&W app and started using the new OS Data Hub Maps API.  This is when my problems really began.

## Stepping into the new world
The first and most significant issue I encountered with the new Maps API was that the maps at zoom levels 0 & 1 for the Leisure style had changed, and were worse than useless.  I'd include a screenshot here if I could, but because the Data Hub has been shut down and no alternative has been provided I'm afraid I can't do that (more, much more, on this later).  The reason the tiles at these zoom levels are terrible is that they're so pixellated that it is literally impossible to read most of the place names indicated on the map.

>I'll repeat that in case you missed it.  **The National Mapping Agency of the UK appears to have difficulty producing raster map tiles of the UK that are legible at zoom levels 0 & 1**

I thought I must have done something wrong, so I went to the examples on the Data Hub website (I'd show you a screenshot or give you a URL to look for yourself at this point if it weren't for the fact that these pages no longer seem to exist and have been replaced by a holding page for the new Data Hub). I found a relevant example, zoomed out and, sure enough, zoom levels 0 & 1 were illegible.

To get around this, I have to use the Outdoor style for zoom levels 0 & 1 (which, mysteriously, does seem to be legible) and then switch to Leisure style for subsequent zoom levels.  Not ideal, but at least workable.

The next issue that immediately became apparent was that the performance of the new API was significantly degraded compared with the old API.  Where before I was able to use [Leaflet.js][leaflet] to animate 'flying' from zoom level 0 to zoom level 9 and have the map update smoothly, now I was finding that the new API couldn'r return the map tiles fast enough to update at all.  All I could see as the map was panning and zooming was a blank background.  As a consequence, I had to roll back a feature I'd added to W&W of flying to a location when the marker was clicked and simply jump to it.  Oscar was not pleased.

 > Why does the performance of the Data Hub Maps API appear to be much slower than the old Maps API?


## Everything falls apart
Here's when the trouble really starts. On Wednesday 17th June 2020 I received an email from the OS Data Hub team informing me that:

```
The OS Data Hub beta trial ends today, Wednesday 17th June.

Please remember all accounts will close when the beta trial ends.

From 1 July you can sign up to your chosen plan of either
OS OpenData or Premium, via our simple sign-up process.
```

This is where I have to admit some culpability for what follows.  I adknowledge that the email clearly says `all accounts will close` but I made the mistake of assuming that an alternative would be provided.  I have been a professional software developer for over 30 years, and in all that time I have *never* experienced a beta product that was simply switched off before an alternative has been provided. I thought perhaps I had just been lucky so I canvassed my colleagues.  None of them have ever used a beta product that was just switched off without an alternative either.

Unfortunately, on Thursday 18th of June Oscar was due to demonstrate his app to a panel of investors.  For some reason, despite being told the trial would end on Wednesday 17th the service continued to be up into the morning of the 18th.  Then, 1 hour before Oscars presentation to *investors* the service went down, taking Oscar's product with it.

 > The shutting down of the Data Hub rendered Walks & Waterfalls useless 1 hour before an investor presentation
 
I jumped into action trying to help Oscar resolve this issue in time for his meeting.  I wish I could say the same of the Data Hub team.  I put a call on Yammer for help and received a series of responses to the effect of `We sent out emails telling you it was going to stop`.  Needless to say, this was of no use to me at this particular time. What I would have preferred is a series of responses saying `How can I help resolve this awful situation for you`, but unfortunately I didn't get that.

I'll spare you the gory details of how I tried, in that remaining hour, to find an alternative for Oscar.  Suffice it to say that no alternative was forthcoming and I had to advise Oscar to cancel his demo 20 minutes before it was due to be given.  Did I mention that this was a demonstration to *investors*? I'll leave it up to you, dear reader, to imagine how well that conversation went.

In the 40 minutes I had available to try something, anything to help Oscar before having to cancel his meeting I did manage to contact OS Service Desk who provided me with an API key for the 'old' API that I was using prior to the Data Hub.  Unfortunately that key didn't work! I tried using my old API key.  That didn't work either! I tried logging in to the OS Developer Portal for which I have a login.  Unfortunately, the OS Developer Portal refused to recognise my email address, and told me I didn't have an account! So I tried to create a new account.  When it came to registering that new account I set my username, only to be told that this username was already in use!  Presumably by the old account that I'd just been told didn't exist!  At this point I gave up and told Oscar to cancel his meeting.

While I was doing this I asked Oscar to sign up for a Developer account.  The screenshot below will give you some idea of how well that went.

| ![signup image](https://user-images.githubusercontent.com/2972038/85549214-ab243000-b617-11ea-9e0a-060aa0b12387.png) |
|:--:|
| **Figure 1: The 'simple' signup process** |

I don't know if this is the same `simple sign-up process` mentioned in the emails advertising the re-opening of the Data Hub or not. Either way, I think this demonstrates that it is anything but simple to sign up!

Later that day I was contacted by a member of the OS Data Hub team who had the good grace to apologise for any inconvenience caused and offered me his API key to the 'old' API.  For whatever reason I couldn't get it to work (I think the fault was with me, not the key).

It was not until 11:17 on Friday 19th June that a member of the Data Hub 'officially' reached out to offer help - **24 hours after it would actually have helped**.  I was eventually able to get the 'old' API running for Oscar, and he is now back in business.

This entire experience has left me feeling unable to recommend the OS Data Hub to any of our startups even when it re-starts on 1st July. I cannot, in good conscience, recommend this service until it has been up and running for at least 6 months and has been shown to have resolved all of the issues I have outlined above.

 [leaflet]: https://leaflet.js


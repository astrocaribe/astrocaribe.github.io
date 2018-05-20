---
layout: post
title:  "Adding Slack Messaging To Report Errors"
date:   2018-05-19 10:00:00
author: astrocaribe
category: dev
tags: [python slack logging errors]
---

## Beer Me!
As a way to practice coding on my software development journey, I have embarked
on experimenting with the Raspberry Pi platform. I am quite fond of beer, and I
quite enjoy the art of brewing. In this process, the beer needs to be stored for
weeks at a time, preferably in a location with a specific temperature range.

## Raspberry Pi Zero and MCP9808
And it was thus that I thought of monitoring a spare closet with a Raspberry Pi
Zero and an [MCP9808][mcp9808] temperature sensor breakout board. The Pi is
lightweight, requires minimal power, and the Zero has the bonus WiFi chip. So, I
can stick it in the closet with a constant power supply, and basically leave it
on for all eternity, if necessary. You can find out a little more about this
project [here][beer-monitor]. (**Note**: *This is still very much a work in
progress*)

I finally got the code to place where I could indeed leave it on for days/weeks
at a time. However, it was very fragile; basic errors (inability to communicate
with a service to which it reports, or errors communicating with [DarkSky][darksky],
where I get local temperature data from, amongst other errors) would cause the
monitor to crash and burn, and sometimes it would be a few days before I noticed.
How can I fix this?

## Crash And Burn, Baby!
Ultimately, the problem was bad coding on my part; ugly flow within a try/catch
and lack of proper logging. Most of the time, I'd never see the error itself,
so I had no idea what was taking down the monitor. I decided to set up a
[Slack][slack] bot to figure this out.

## Slack Messaging To The Rescue
Start by going to the [Slack API][slack-api] to create a new app
<sup>[1](#footnote1)</sup>; and hitting that green button "Create New App"
<sup>[2](#footnote2)</sup>. Enter an App Name, select an available workspace,
then create that app!

There is a slew of options to setup and configure your app (bot); experiment to
your hearts content! Don't forget though, the ultimate goal is to have the
script tell you when something fails, at the moment that it fails. If the failure
results in my script burning, then I can quickly restart (as a stop-gap, at the
very least). Under the "**Features**" section of your app, select
"**Incoming Webhooks**". Near the bottom of the panel that is displayed is a
**Webhook URL** that you can copy. This is the URL you want to use when sending
a message to Slack from within your app. There is even a sample request that you
can try to make sure it indeed it works! Just make sure to create the webhook
first, and link it to an existing channel (public or private).

## In Closing ...
This writeup is way too short to describe everything that had to be done in
terms of setup. However, it is all fairly straightforward and shouldn't cause
too much heartache. For a real use case, check out my [implementation][beerme]
that saved my python app from constantly crashing!

Happy Coding!


---
<a name="footnote1">1</a>: Assumes that you have a Slack account with access
to a workspace.

<a name="footnote2">2</a>: You'll also be required to agree to the
[Slack App Directory Agreement][slack-terms] terms.

[mcp9808]: https://www.adafruit.com/product/1782
[beer-monitor]: https://github.com/astrocaribe/Beer-Temp-Monitor
[darksky]: https://darksky.net/
[slack]: https://slack.com/
[slack-api]: https://api.slack.com/apps
[slack-terms]: https://api.slack.com/slack-app-directory-agreement
[beerme]: https://github.com/astrocaribe/Beer-Temp-Monitor/blob/master/src/beer_temp_mon.py

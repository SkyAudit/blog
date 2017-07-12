+++
title = "Development Update #120"
tags = [
    "Development",
    "Infographics",
    "CXO",
    "Marketing",
]
date = "2016-12-25"
categories = [
    "Development Update",
]
description = "Release notes highlighting the current development behind Skycoin."
+++

###### We are at 90% launch readiness:
- Network is operating
- 3 of the 7 white papers are on the websites
- Deposits, withdrawals and done
- Skycoin order book is done
- CLI and WebRPC are done
- Skycoin.net website is 70% done (backlog of tickets)
- Over half the wallet builds are on website (but have backlog of UI bugs)
- Wallet explorer API is done. We are waiting on web dev to fix UI bugs and to do the stand alone blockchain explorer
- Infographics are done
- Introduction video is in progress
- Several of our applications are close to launch (but backlog)

## Infographics

![](http://i.imgur.com/hQKAhdL.png)
![](http://i.imgur.com/8YvFOcH.png)
![](http://i.imgur.com/mXKMANO.png)
![](http://i.imgur.com/cwl95f5.png)
![](http://i.imgur.com/kTHUEk5.png)
![](http://i.imgur.com/oklJWfj.png)
![](http://i.imgur.com/2OWXQCY.png)
![](http://i.imgur.com/5uT1jol.png)
![](http://i.imgur.com/WBahBBc.png)

## CXO

We did benchmark two days ago and were able to sync 20,000 hash objects in 10 seconds.

The meshnet is being refactored, so that it can be run and debugged in a single threaded context, to avoid the use of mutex and multiple redundant goroutines as much as possible.

After we get the coin backlog done, then we will switch to marketing and launching applications.

## Marketing

We are starting with dedicated marketing and intend to do one content push per channel per week.

We will move the development and technical discussion to our own distributed social media platform on top of CXO as soon as possible.

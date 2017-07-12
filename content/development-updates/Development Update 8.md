+++
title = "Development Update #8"
tags = [
    "Development",
]
date = "2014-03-16"
categories = [
    "Development Update",
]
description = "Release notes highlighting the current development behind Skycoin  "
+++

We are compiling binaries for the testnet. Will release binaries and instructions for running testnet from source. You will be able to get testnet coins by posting address in github thread.

### Question:
Quote from: **td services** on March 17, 2014, 05:19:59 AM
>Will these run on Windows, or can they run on linux vps?

Yes. We should have windows binaries.

##### Notes:
- If you want to run from source, linux is much easier than windows, because windows requires dealing with mingw configuration.
- When golang 1.3 comes out, we will be cross compiling the windows binaries from linux directly
- We are looking into using docker for deployment and testing
- 64-bit is highly recommended
- Some people are getting errors because golang is not detecting changes to imported modules correctly. ./compile/clean-static-libs.sh fixes this problem by clearing out the cache


---
title: "zmk dongles"
date: 2024-05-14-T00:00:00-05:00
draft: false
categories: [ daily ]
tags : [ blog, zmk, keyboards ]
---
I am trying to create a dongle setup to use with my wireless skeltyls.   I have not been using them as much as i would like because they are a giant pita to deal with when they come out of sleep, and when they are reconnecting.   so i thought that i would like to try and do a dongle mode with them and see how it goes.


Working with ZMK dongles mode in ZMK I have a ZMK keyboard. I have many ZMK keyboards. Right now I have two different skeletal keyboards and they are all using nice nano BLE boards on them and the Bluetooth connectivity, once it gets connected, it is fine. However, when the Mac goes to sleep or when the keyboard goes to sleep, getting both halves reconnected to the Mac is difficult and annoying and then hopping between computers is doubly difficult because I forget which computer is under which BLE profile and then reconnecting, disconnecting, forgetting, clearing the bonding, etc. It's super, super annoying. So the way around that is to use a separate BLE board as a BLE dongle and that BLE dongle then needs its own firmware and its own connection information. So I had a number of seed do we know BLE boards and I found out that we could use dongles for that or use that in a dongle mode. So I took the firmware and using these configuration pinouts and configuration settings for the keyboard we can then create a BLE dongle with the seed do we know board and use that to connect to the nice nano boards. Simple and relatively painless and that is that.
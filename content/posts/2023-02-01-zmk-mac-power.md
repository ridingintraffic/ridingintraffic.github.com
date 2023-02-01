---
date: 2023-02-01T00:00:00-05:00
draft: false
title: 'wireless zmk mac sleep waking bug'
categories: [ daily ]
tags : [ life ]
---

# bug in zmk
fancy keyboards use a controller called a nice nano,   that nice nano runs a firmware called zmk.
That firmware has a fair few options to configure a few things.

One of the nice options is deep sleep, which powers things down to save battery on the board when not in use.
Many people argue that this feature eliminates the need for a physical battery switch.
However there is a bug with mac that is/was waking up the mac from sleep eventhough the "keyboard is alseep"
You can fix it with.
```
CONFIG_ZMK_SLEEP=y # Enable deep sleep support
CONFIG_ZMK_IDLE_SLEEP_TIMEOUT=900000 # Milliseconds of inactivity before entering deep sleep
```
open issue about the bug
https://github.com/zmkfirmware/zmk/issues/1273
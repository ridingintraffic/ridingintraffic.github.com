date: 2023-02-09T00:00:00-05:00
draft: false
title: 'macos text entering weirdness'
categories: [ daily ]
tags : [ life ]
---
Just another basic reminder for a thing that I fixed with my laptop when it was acting odd and weird
I was working on my mac and I kept encouring a weird behavior where certain keys that were held down would not repeat, but would pop up a prompt on my screen to enter some kind of special accented internantionl character.
In macOS, when a key is held down while entering text, a popup is shown which lets one choose between various accented forms of the character. To disable this execute the following command line in the Terminal.app:
`defaults write -g ApplePressAndHoldEnabled -bool false`
Now, you'll need to log out and log back in. This should disable the display of the popup and character typed should start repeating when the key is held down.
If you ever wish to return to this behaviour, execute the following command line in the Terminal.app:
`defaults write -g ApplePressAndHoldEnabled -bool true`
You'll need to log out and log back in again for the setting to take effect.

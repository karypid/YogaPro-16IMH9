# YogaPro-16IMH9
Running Linux on the Yoga Pro 9i - 16IMH9 - 2024, Intel, Gen 9

## Installers of popular distros

- Ubuntu 23.10: installer does not allow you to connect to network
- Fedora (Workstation/Silverblue) 39: installer does not allow you to connect to network
- Fedora (Workstation/Silverblue) 40: installer works fine **with Intel GPU**, and has network WiFi support

## Fedora Silverblue 40

I ended up using Fedora Silverblue 40 (beta) with Gnome 46. The only issue was that I had to go into the BIOS and switch the GPU from "dynamic mode" to "Intel UMA" to force use of the Intel GPU. After installation was complete, I used rpm-ostree to install the Nvidia drivers and was able to switch back to dynamic mode.

Almost everything works so a very good experience. See below for minor sound/keyboard issues. Here is the kernel I've tested with:

```
~# uname -a
Linux myhostname 6.8.4-300.fc40.x86_64 #1 SMP PREEMPT_DYNAMIC Thu Apr  4 20:41:39 UTC 2024 x86_64 GNU/Linux
```

### Things that "just work"

- Camera
- Keyboard (even almost all of the special keys, see below)
- Touchpad
- Thunderbolt port
- Intel GPU

### Things with minor quirks

- Nvidia GPU: had to disable it in BIOS to complete installation. Once the system was up and running I installed the drivers following [these instructions form RPM Fusion](https://rpmfusion.org/Howto/NVIDIA#OSTree_.28Silverblue.2FKinoite.2Fetc.29) and after that I was able to re-enable the card in BIOS and it works fine.
- Sound: it seems like the driver only enables the twitters and keeps the main speakers off, as a result you hear only the higher frequencies at a low volume. This is [a known bug](https://bugzilla.kernel.org/show_bug.cgi?id=217449) that is already addressed (newer kernels will "just work") and has an easy workaround. See [this attached script](https://bugzilla.kernel.org/attachment.cgi?id=304763) in that bug. I run it on boot using "2" as the parameter (that is the bus number on my model). So a simple `sudo ./2pa-byps.sh 2` fixes it and the sound is amazing.

### Keyboard special keys

Almost all of them work:

- Volume mute (alongside F1): as expected
- Volume down (alongside F2): as expected
- Volume up (alongside F3): as expected
  - Microphone switch (alongside F4): **seems to not work**
- Brightness down (alongside F5): as expected
- Brightness up (alongside F6): as expected
  - Monitor switch (alongside F7): works but not very user-friendly (see below)
  - Aeroplane mode (alongside F8): **seems to not work**
- The "sun" like icon (alongside F9): not sure what it's for, but **seems to not work** as it does nothing
- Lock screen (alongside F10): as expected
- Calendar (alongside F11): as expected
- Calculator (alongside F12): as expected
- Bookmarks/favorites (alongside Insert): in Firefox as expected, brings up the bookmarks pane
  - The "scissors" like icon (alongside PrtSc): not sure what it's for, but **seems to not work** as it does nothing

(*): the monitor switch button works, but it seems to me Gnome 46 has a bit of a usability issue. When you press it, it does indeed cycle between "mirror on both screens / extend across screens / external only / internal only" modes. The problem is that it switches mode **immediately** when pressed. As a result the screens go black to re-sync and you don't really get a chance to see which mode was just picked. I would expect there to be a delay, so you can press the key again to move on to another mode. This way you can move to waht you actually want to activate and have the mode switch once (and thus the monitors re-sync once).

### Internal Monitor

I have this panel option:

> 16" 3.2K (3200 x 2000), Mini-LED, Anti-Glare, Non-Touch, HDR 1000, 100% Adobe RGB, 100%DCI-P3, 1200 nits, 165Hz

It gets **very** bright indeed. When in the dark (e.g. watching a movie at night with lights off) I turn brightness all the way down, one step before absolute minimum (which is completely off).

**For some reason Night light does not work**. It seems to do nothing. This is very weird because when connecting my external monitor with both screens enabled it works for the external monitor but the panel on the laptop is not affected.

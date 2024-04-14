# Remap OMEN key to Home

Remap the OMEN key to Home for HP OMEN laptops.

The Home key on HP OMEN laptops is replaced by a special key that launches the Command Center application. It is not recognized as a keyboard key by Windows. 

It is possible to change the function of this key back to Home, by putting a small program in place of the Command Center link, which simulates the Home key to the operating system.

This branch edits the registry to make the driver start the simulating program. There is [another approach](https://github.com/jingyu9575/remap-omen-key/tree/custom-client) which replaces part of the driver and is more responsive, but it requires importing a certificate. 

## Guide

First, the driver ["HP System Event Utility"](https://ftp.hp.com/pub/softpaq/sp101501-102000/sp101543.exe) (with `HPMSGSVC.exe`) must be installed.

Download [send-home.exe](https://github.com/jingyu9575/remap-omen-key/releases) and put it in a fixed location.

Run `regedit`, navigate to `HKEY_CURRENT_USER\Software\HP\HP System Event\Bezel Button\8613`, and change the value of `ApplicationPath` to the path of the downloaded program.

## UAC Issues

By default, the auto-started HP Message Service does not run with the Administrator privilege, so when an elevated application is active, the remapped OMEN key may not work.

To resolve this issue, run `taskschd.msc` and create a basic task:

* Name: HP Message Service
* Trigger: When I log on
* Action: Start a program
* Program: `"C:\Program Files (x86)\HP\HP System Event\HPMSGSVC.exe"`

Double-click the created task and enable "Run with Highest Privileges".

The default unprivileged auto-start can be disabled in the Start-up tab of Task Manager.

---

## HP OMEN Sequencer Keyboard

This is an external keyboard with the same issue. Instead of a custom system event, the OMEN key on this keyboard sends the standard F24 key code (see #2). It can be remapped to Home by running the following AutoHotkey script in the background:

```autohotkey
#NoTrayIcon
F24::Home
```

Download the compiled executable: [F24ToHome.exe](https://github.com/jingyu9575/remap-omen-key/releases)

# Advanced Audio Setup for Rocksmith 2014 Remastered on Linux

This is an advanced guide for Linux audio in general which utilizes many different packages and tools.

## Requirements

>   * WineASIO [*https://kx.studio/Repositories*](https://kx.studio/Repositories)

>    * RS-ASIO [*https://github.com/mdias/rs_asio*](https://github.com/mdias/rs_asio)

>    * Jack2 audio server [*https://github.com/jackaudio/jack2*](https://github.com/jackaudio/jack2)

>    * QJackCtl jack control panel, which usually contains Jack2

>    * Wine/winetricks

>    * Protontricks (optional) helps with setting up various wine settings for proton steam installs through things like winetricks. --gui command helps a lot.

---

## 1. Software installation

__Install KXStudio repositories via deb file from this link:__

>[https://launchpad.net/~kxstudio-debian/+archive/kxstudio/+files/kxstudio-repos_10.0.3_all.deb](https://launchpad.net/~kxstudio-debian/+archive/kxstudio/+files/kxstudio-repos_10.0.3_all.deb)

__Or manually install (debian/ubuntu):__

>[https://kx.studio/Repositories](https://kx.studio/Repositories)

__Install applications via terminal__

`sudo apt install rtirq grub-customizer jackd qjackctl wineasio`

Make sure to fix any dependency issues before continuing

__Enable "threadirqs" in kernel/grub (Possibly not needed)__

Use a tool such as grub customizer or manually change in various distros with parameters as such:
`quiet splash mitigation=off threadirqs`

## 2. Installing and setting up Jack2 audio server

Open QjackCtl from the start menu and click on Setup. On the first tab click on the interface dropdown and select the guitar interface you have connected to your USB and set the following:

    Realtime: checked

    Sample Rate: 48000

    Frames: 512 ( will set lower once everything works)

    Periods: 3

Hint: Do not use different devices for input and output. Your guitar interface should act as input and as output. Having different inputs and outputs can lead to bad things such as buffer underruns (xruns) and stuttering/system stability issues.

Go to Advanced. On the "Input Device" on the right side you set the guitar interface again. Tick the H/W Monitor box. Go to misc and check the same boxes as the screenshot below:


<img src="/img/qjackctl.png">

Click OK to save your settings and then click the start button on QJackCtl to start the audio server.

<img src="/img/qjackctl2.png">

Also click the messages button to see the console output/logs for the audio server. If there are any errors, you may want to research them and address them now.

## 3. Installing wineASIO and SteamPlay integration with RS_ASIO

Copy the file wineasio.dll.so from /usr/lib/i386-linux-gnu/wine to the wine directory of the Proton version you use for Rocksmith. Unless you have custom game libraries, Proton is located here: __/home/yourusername/.steam/steam/steamapps/common/Proton 5.13/dist/lib/wine/__. 

Debian installs are usually located at __/home/yourusername/.steam/debian-installation/steamapps/common/Proton 5.13/dist/lib/wine/__

__Warning:__ If your Proton version changes, you will most likely have to do these steps again.

Register wineasio with the Proton instance of Rocksmith via terminal. 221680 is the steam id of RS2014 Remastered:

`WINEPREFIX=/home/yourusername/.steam/steam/steamapps/compatdata/221680/pfx regsvr32 wineasio.dll`

Configure wineasio in Proton:

`WINEPREFIX=/home/yourusername/.steam/steam/steamapps/compatdata/221680/pfx winecfg`

Alternatively, use protontricks to open winecfg for your game install.

In the wine configuration window go to the libraries tab and add the wineasio.dll as a new override. Edit it as native, builtin.

Configure wineasio via registry:

WINEPREFIX=/home/yourusername/.steam/steam/steamapps/compatdata/221680/pfx regedit

Protontricks can also be used like before to do this.

Go to the key HKEY_CURRENT_USER\Software\Wine\WineASIO and configure the values as follows:

    Autostart server : 1

    Connect to hardware : 1

    Fixed buffer size : 1

    Number inputs: 2

    Number outputs: 2

**TIP:** If these registry keys do not already exist, you can manually create them.

Setup and configure rs_asio as per it's Github page.

## 4. Testing

* Start the jack2 server via QJackCtl and open the message window to see what happens.

* Now start Rocksmith.

> **Hint:** Whichever device is used in Jack will be used as a Realtone cable through RS_Asio. If the game detects a Realtone cable, you are heading in the right direction.

* In the messages window of QjackCtl you should see hints that RS tried to connect

* Click on Connect in the QjackCtl panel, you should see the Rocksmith inputs and outputs there

> **Hint:** Rocksmith's sound will most likely become muted when you leave the game window.

* If you are hearing stuttering/noise issues, close the game and try increasing your sample rate/frames in Jack settings. 

* If you are not hearing any audio issues, close the game and try decreasing your sample rate/frames in Jack settings as far as you can to reduce latency.

## Sources

###### This tutorial was adapted from other tutorials/forum posts online which can be found in the links below.

[Main Reddit Guide](https://www.reddit.com/r/rocksmith/comments/jxngpx/howto_rocksmith_2014_on_linux_with_steamproton/)

[RS_ASIO/WINEASIO Github Issue](https://github.com/mdias/rs_asio/issues/99)

---

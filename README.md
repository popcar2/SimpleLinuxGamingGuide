# Simple Linux Gaming Guide

<h3>An In-Depth Guide for Running Windows Games on Linux, Made for Beginners</h3>
<sub>Originally published at https://popcar.bearblog.dev/everything-linux-gaming/</sub>

---

## Table of Contents
1. [Introduction](#1-introduction)
2. [Status of Linux Gaming](#2-status-of-linux-gaming)
3. [Note on NTFS File Systems](#3-note-on-ntfs-file-systems)
4. [Double check your Nvidia drivers](#4-double-check-your-nvidia-drivers)
5. [**Gaming on Steam**](#5-gaming-on-steam)
    1. [Installing Steam](#i-installing-steam)
    2. [What is Proton?](#ii-what-is-proton)
    3. [Enabling Proton](#iii-enabling-proton)
    4. [Installing Proton-GE (Optional)](#iv-installing-proton-ge-optional)
6. [**Gaming outside of Steam**](#6-gaming-outside-of-steam)
    1. [Introducing Heroic Games Launcher](#i-introducing-heroic-games-launcher)
    2. [Installing Wine-GE](#ii-installing-wine-ge)
    3. [Introducing Wine Prefixes](#iii-introducing-wine-prefixes)
    4. [Running Epic/GOG/Amazon games on Heroic Launcher](#iv-running-epic-gog-amazon-games-on-heroic-launcher)
    5. [Running other games on Heroic Launcher](#v-running-other-games-on-heroic-launcher)
    6. [Giving Heroic Launcher file permissions (Optional)](#vi-giving-heroic-launcher-file-permissions-optional)
7. [Closing thoughts](#7-closing-thoughts)

X. [Appendix](#appendix)

---

# 1. Introduction

Welcome! This is an in-depth guide on everything you need to know in order to get into Linux gaming. Note that this is *not* going to cover choosing a Linux distro or installing Linux in general. For the sake of this post, I will assume that you already have Linux set up and are ready to start gaming.

Feel free to skip around the table of contents to get where you want to be quicker. There's also an appendix at the bottom if you ever get lost in weird Linux terminology and need a refresher.

Some of you may wonder: **Why another guide?** Well, a lot of other guides often leave out important info and talk too much about trivial things. I made a few mistakes and had to collect info scattered around the internet that would sometimes be outdated over my time playing games on Linux. My hope is to make one long post, explaining from start to finish the why and hows of Linux gaming while being beginner-friendly and not too technical. 

# 2. Status of Linux Gaming

As of 2023, Linux gaming is actually in a *very* viable state. Most Windows games run without needing any tinkering, and you can usually expect smooth sailing, especially if the game you want to play is on Steam.

The only real holdover from Linux gaming being as good as Windows is anti-cheat. Many multiplayer games such as Rainbow Six: Siege, Valorant, Destiny 2 and more do not support Linux yet. That said, the situation *is* improving, with many developers opting in to provide Linux support despite running Anti-cheat. Games like Elden Ring & Apex Legends run well out of the box. Hopefully more games will be playable over time.

As for performance, while I don't have any hard statistics, I can confidently say that you should expect performance to be *similar* to Windows, though usually slightly worse. Expect a minor performance hit as a result of running them through a compatibility layer. 

A few years ago I would say gaming on Linux isn't worth it. I used ZorinOS for a while but often the games I played had issues or randomly broke, especially when gaming outside of Steam. Coming back to Linux recently, I can say the experience has really improved a lot, to the point where I can recommend it to people who aren't technical. That doesn't mean it's *perfect*, but it is certainly not bad at all.

If you need to see which games run well on Linux and which don't, check out [ProtonDB.](tab:https://www.protondb.com/)

<h1 id="3-note-on-ntfs-file-systems"> 3. ⚠️ Note on NTFS File Systems ⚠️ </h1>

It's common that people new to Linux would be dual-booting Windows or generally have their games on a partition created by Windows. The file system Windows uses, called NTFS, sadly causes a lot of issues in Linux gaming. Trying to run games on an NTFS partition will usually cause issues, if they even work at all.

There are many reasons for this, but they mostly boil down to NTFS not being super compatible with Linux. People have found some workarounds to this but I **highly** recommend moving your games out of an NTFS partition. As someone who's been down this route, it's not worth trying to hammer your way through it.

Luckily, Steam has a convenient way of moving games to your Linux partition. You could also move your games to an external hard disk, reformat your partition to something Linux-friendly, then put them back. The latter is what I did, and I recommend doing so to save yourself some heartache.

<sub>**Edit:** A lot of people have messaged me saying they've had better luck than me and got NTFS working properly with their games by following [Valve's official guide.](tab:https://github.com/ValveSoftware/Proton/wiki/Using-a-NTFS-disk-with-Linux-and-Windows#preventing-ntfs-read-errors) It's sort of technical and I still **wouldn't** recommend it (especially with reports that booting into Windows could mess everything up again), but it *is* possible if you really really need to keep your games on an NTFS drive.</sub>

# 4. Double check your Nvidia drivers

Ignore this section if you don't have an Nvidia graphics card.

A lot of Linux beginners often make a mistake of using the open-source Nvidia driver, called Nouveau. Nouveau drivers have terrible performance compared to the official Nvidia drivers, but many distros don't install the official drivers by default because they're not open source.

Open the `Nvidia X Server Settings` application or write `nvidia-smi` in the terminal. If those don't work, try `inxi -b`. If the driver says something like `Nvidia: 535.98` then you're good to go. If it says Nouveau then look into installing the correct drivers for your distro. You will **not** be able to play games properly without them!

<div align="center">
<sub>Here's an example of official Nvidia drivers</sub>
<br>
<img src="https://i.vgy.me/0XQ7Fk.png" alt="Screenshot showing nvidia-smi outputting the Nvidia driver version."/>
</div>

# 5. Gaming on Steam

### I. Installing Steam

With all of that nonsense out of the way, time to install Steam using your distro's package manager. Open your terminal and follow along:

* If you're using Ubuntu (or Debian, ZorinOS, PopOS, Linux Mint, ElementaryOS): Type `sudo apt install steam`

* If you're using Fedora: Type `sudo dnf install steam`

* If you're using Arch Linux (or Manjaro, EndeavourOS): [Follow the Arch Wiki](tab:https://wiki.archlinux.org/title/Steam#Installation)

The terminal will ask for your password. While it looks like you're not writing anything, you actually are! The terminal hides your inputs for security reasons. Just write it normally and press enter. If it asks for conformation, just type `y` and hit enter.

And you're done! Steam should show up within your apps, simply start it and log in.

⚠️ **Note:** I don't recommend installing the Flatpak version of Steam. It has some issues specific to Flatpak, and makes your Steam files difficult to reach which can be annoying. Use your package manager instead.

### II. What is Proton?

Before we get to installing games, we first need to understand what's even going on. The short of it is that there exists an amazing program called **WINE** that allows running Windows programs on Mac & Linux. Wine is awesome and is still in constant development, but it sadly isn't that great at running video games.

Valve (creators of Steam) wanted Windows games to run on Linux anyways, so they took Wine and created a version of it specifically for video games, called **Proton**! This is what Steam uses to run Windows games on Linux, and it works beautifully.

### III. Enabling Proton

In order to enable Proton on Steam, click on Steam in the top left --> settings. Then, go to Compatibility and turn on `Enable Steam Play for supported titles` and `Enable Steam Play for all other titles`. 

In the `Run other titles with: ` row, you get many versions to choose from. Generally you should choose the most recent stable version (as of writing it is Proton 8.0-3). Proton Experimental is a beta build that may work better but also may break other games. Only use it when necessary.

![A screenshot showing the step-by-step process to enable Steam Play (Proton)](https://i.vgy.me/q2umsM.png)

Steam will prompt you to restart the app. Go ahead and do so. When you open back up, you now have access to all your Steam games. Just download and run as you would normally, and after a bit of install time they should just work!

Again, be sure to check out [ProtonDB](tab:https://www.protondb.com/) to see which games run well, and which might not work.

The final thing you need to know about running games on Steam is that every time you play a game for the first time, Steam creates ~250mb of files inside of `Steam/steamapps/compatdata/[Game ID]`. This is called a **prefix** folder. I'll talk more about this [in section 6.3](#iii-introducing-wine-prefixes), and while you usually don't need to mess with them, it's worth noting so you don't get surprised that installing many small games can use a lot of space.

### IV. Installing Proton GE (Optional)

Another thing you need to know is the existence of Proton-GE, an improved version of Proton that is patched with various fixes and improvements. Often if a game has issues with regular proton or you need to squeeze a little bit more FPS, it's preferable to use Proton-GE. There are usually zero downsides to using it, so let's use it. 

To install Proton-GE, I recommend a handy program called **ProtonUp-Qt** that will help us manage our Proton versions. You can install it [as a Flatpak](tab:https://flathub.org/apps/net.davidotek.pupgui2) or just [download it from GitHub](tab:https://github.com/DavidoTek/ProtonUp-Qt/releases) (you want to download the `.appimage` file).

Once downloaded, open it up and it should automatically detect Steam. Click on "install version", choose the most recent Proton-GE version, then click install.

<div align="center">
    <img src="https://i.vgy.me/13NBVI.png" alt="Image showing the Proton-GE installation process via ProtonUp-Qt" />
</div>

And you're done! Restart Steam if you have it open, then go to `Steam --> Settings --> Compatibility` and you should be able to choose Proton-GE as your default. Your games should work slightly better now.

<div align="center">
<img src="https://i.vgy.me/lFSF8m.png" alt="Image showing Proton-GE selected as the default on Steam" />
</div>

---

# 6. Gaming outside of Steam

Adding a non-steam game into Steam and trying to run it is **not** recommended and often causes a lot of issues. Because of this, running non-Steam games is a bit more complicated. Luckily, there are tools that make things much simpler.

Which program you use for running games outside Steam is pretty opinionated. For the sake of this tutorial I'll use [Heroic Games Launcher](tab:https://heroicgameslauncher.com/) since I think it's the most convenient and simple to use.

Other popular programs that do roughly the same thing are [Lutris](tab:https://lutris.net/) and [Bottles](tab:https://usebottles.com/). Honestly, I like both of them too, but they're a bit beyond the scope of this guide.

### I. Introducing Heroic Games Launcher

Heroic Games Launcher was originally an open source launcher for the Epic Games store, then the GOG store. Over time, they added more and more features to support running all sorts of games, similar to Lutris and Bottles.

Unlike Steam, I *do* recommend downloading the Flatpak version of Heroic Launcher. You can [download it from FlatHub.](tab:https://flathub.org/apps/com.heroicgameslauncher.hgl) Most modern distros have Flatpak pre-installed, but if for whatever reason you don't, you can install it via your package manager.

Flatpaks may take a while to install, but when you're done simply open the launcher and you should be greeted with this screen:

![A screenshot of Heroic Games Launcher.](tab:https://i.vgy.me/RmK9Ky.png)

### II. Installing Wine-GE

Proton is created *specifically* for gaming on Steam. Attempting to use it outside of Steam can cause issues, so the community instead uses Wine-GE. Wine-GE is *very* similar to Proton, and you can expect similar performance and compatibility, except it's easier to run outside of Steam.

You can install Wine-GE by clicking on `Wine Manager` on the left, then hit the download button on the version you want to use. Download the most recent version of `Wine-GE-Proton`. As of writing this, the most recent one is `Wine-GE-Proton8-13`.

Alternatively, you can download Wine-GE through ProtonUp-Qt, which I mentioned [in the section about Proton-GE above](#4-installing-proton-ge-optional). You really don't have to, though. 

You may notice alternate versions like `Wine-GE-LOL`; these are for specific games such as League of Legends. Don't use those unless you need to.

<sub>Wine-GE is maintained by Glorious Eggroll by the way, the same person who maintains Proton-GE.</sub>

### III. Introducing Wine Prefixes

Wine prefixes are an important concept you have to be familiar with in Linux gaming. A wine prefix is essentially a large folder that mimics a Windows system, with its own `C:\` drive, program files, registry files, etc.

By default, every single game you own would be in its own Wine prefix. The reason for this is to "sandbox" each game so that if something broke in one "Windows system", all your other games would be fine. It also allows you to make changes to a specific Prefix without worrying about it affecting more than one game.

The only real downside to this is that each Wine prefix uses roughly ~250mb of storage space by default. It's a small price to pay for stability, but if you want to save some space you *can* go the "chaotic neutral" route of cramming your games in one Prefix. It isn't recommended to do so, but it's doable if you know what you're doing.

<sub>Note: Wine Prefixes are also where your save files are stored. If you need to backup or import a save file, look there!</sub>

<h3 id="iv-running-epic-gog-amazon-games-on-heroic-launcher"> IV. Running Epic/GOG/Amazon games on Heroic Launcher </h3>

<sub>Note: It's recommended to check [ProtonDB](tab:https://www.protondb.com/) and see if your game runs well on Linux before installing.</sub>

Now that you (sort of) understand what a Wine Prefix is, let's actually install some games. You have two options on Heroic: Downloading from a store it supports, or manually adding a game you have on your drive. I'll demonstrate both.

Heroic Launcher supports logging into Epic Games, GOG, and Amazon Prime to easily download and install games without any tinkering needed.

Click on `Log In` at the top left and login to whichever store you'd like to use. Your games should now show up in your library. Click on whichever game you'd like to install, I'll go for Enter the Gungeon:

![A screenshot showing the Heroic Launcher installation prompt](https://i.vgy.me/vam5a6.png)

Heroic will automatically create a Wine Prefix for you, so you don't need to edit it. You can select a Wine Prefix that already exists if you know what you're doing. Make sure you have Wine-GE selected similar to the screenshot.

That's it. Your game should install without issue. If you're using a gaming laptop, you may need to right click and go to `Settings --> Other` and toggle `Use Dedicated Graphics Card` on. 

<div align="center">
<sub>Eeeeeeeenter the Gungeon</sub>
<br>
<img width=75% src="https://i.vgy.me/bzdlWy.jpg" alt="A screenshot showing Enter the Gungeon" />
</div>

### V. Running other games on Heroic Launcher

<sub>Note: It's recommended to check [ProtonDB](tab:https://www.protondb.com/) and see if your game runs well on Linux before installing.</sub>

This section is for manually adding games you have on Heroic, whether it be an installer file or a portable exe.

I've got the installer for Risk of Rain with me that I want to install. Click on the `Add Game` button at the top and it'll open a form for you to fill out. Here's what everything means:

* **Game/App Title**: The name of the game.

* **App Image**: The image it will show in your launcher. Heroic will try getting the image automatically via the game title, which is very cool!

* **Platform Version to Install**: Whether this is a Windows, Linux, or browser game. Since this is an exe I'll leave it at Windows.

* **Wine Prefix**: Heroic will automatically create a Wine Prefix for you, so you don't need to change this. You can choose an existing Wine Prefix if you'd like.

* **Wine Version**: This should be set to Wine-GE, which may be called something slightly different (see screenshot below). 

* **Select Executable**: The exe file that runs your game, NOT your installer.

If you want to use an installer, click on `Run Installer First` at the bottom. This will prompt you to select your installer file and run it within your selected Wine Prefix. Since I want to install Risk of Rain, I'll do so.

<div align="center">
<sub>This will install the game in my Wine Prefix</sub>
<br>
<img width=75% src="https://i.vgy.me/CxGIy6.png" alt="A screenshot showing the installer for Risk of Rain" />
</div>

Now I can point to the exe of my installed game. **If you *don't* need to install the game** and already have the exe ready: point to the game, run it once, then move it into its Wine Prefix if it's not working.

<div align="center">
<sub>Your result should look like this</sub>
<br>
<img src="https://i.vgy.me/l4eyJq.png" alt="A screenshot showing the filled out form" />
</div>

Et voila. Hit finish and run your game, it should work! If you're using a gaming laptop, you may need to right click and go to `Settings --> Other` and toggle `Use Dedicated Graphics Card` on.

If for whatever reason your game *doesn't* work, right click the game and click `Logs`. It will help you troubleshoot.

<div align="center">
<sub>...And his music was electric.</sub>
<br>
<img width=75% src="https://i.vgy.me/hL6ZSh.jpg" alt="A screenshot showing Risk of Rain" />
</div>

#### VI. Giving Heroic Launcher file permissions (Optional)

I've talked a lot about how Heroic doesn't have permissions to access most of your files, meaning you usually have to move your games and Wine Prefixes to its folder at `/home/Games/Heroic/` in order for them to work, but there **is** actually a way you can fix this.

To do this, [download Flatseal](tab:https://flathub.org/apps/com.github.tchx84.Flatseal), which is a great program that lets you easily manage your Flatpak permissions. It's only half a megabyte too, which is nice. 

Open Flatseal, select Heroic Games Launcher, and scroll down until you reach `Filesystem`. From here, you can manually add folders or selectively allow access to major parts of your system. Heroic Launcher is open-source, so it's relatively safe to just allow access to major areas of your system. I gave it access to all my user files and called it a day.

Using this you can now throw your Wine Prefixes everywhere, and run games from other drives you might have without having to copy everything over. Very useful.

<div align="center">
<sub align="center"> Finally, freedom...</sub>
<br>
<img src="https://i.vgy.me/IX6wIn.png" alt="A screenshot showing Flatseal in action." />
</div>

# 7. Closing Thoughts

With all the skepticism of Linux being able to play games (or even provide a stable desktop experience), it's reached a point where I feel like I can finally recommend it to most people and not expect them to suffer through a tedious learning process. To all the devs that worked on Linux over the years: Thanks!

This guide took a bit of effort, so if you found it useful and are feeling generous please consider [buying me a ko-fi!](tab:https://ko-fi.com/popcar2)

---

# Appendix

In case you've forgotten what on earth everything means.

* **Linux Distro**: A version of the Linux operating system, like Ubuntu or Fedora.

* **Flatpak**: A modern tool for installing and running Linux applications, usually more stable than their package manager variants because they're sandboxed and rely on their own dependencies. It's quickly becoming a standard.

* **Flathub**: The most popular source of Flatpak apps.

* **Flatseal**: An application that allows you to control the permissions of other Flatpak apps.

---

* **Wine**: A compatibility layer that allows running Windows apps on Linux (and Mac).

* **Wine-GE**: A remix of WINE tuned specifically for playing Windows games on Linux **outside of Steam**. Created by a user called Glorious Eggroll.

* **Proton**: A remix of WINE tuned specifically for playing Windows games on Linux. Used in Steam.

* **Proton-GE**: A remix of Proton with more fixes and enhancements than the default version. Created by a user called Glorious Eggroll.

* **Wine Prefix**: A folder that simulates a Windows folder structure. It's about 250mb, and it's recommended that every game be in its own Wine Prefix.

* **Proton Prefix**: Exactly the same thing as Wine Prefixes, but for Proton. They're stored in `Steam/steamapps/compatdata/[Game ID]`.

* **ProtonUp**: A command line tool for managing and installing alternative versions of Wine and Proton (such as Wine-GE & Proton-GE)

* **ProtonUp-Qt**: A GUI version of ProtonUp. Highly recommended to use this instead.

---

Note: these 3 programs are interchangeable, and mostly boil down to preference.

* **Heroic Games Launcher**: A program that allows you to run non-Steam games easily. Also allows connecting to the Epic and GOG stores.

* **Lutris**: A program that allows you to run non-Steam games easily. It's more feature-packed than Heroic, but more complex to use. 

* **Bottles**: A program that allows you to run and manage Wine apps easily. Can also be used for gaming.

# KDE-Terminal-Wallpaper-Changer
Looking around I found the code needed to change wallpapers in KDE. Unfortunately it is a pain to use in other scripts. 

Here is where I took the code from:
https://store.kde.org/p/1169583/

The purpose? Changing wallpapers from scripts, time based wallpapers... also lock screen wallpaper changing based on time or something else.

### Instalation
```
git clone https://gitlab.com/RaitaroH/KDE-Terminal-Wallpaper-Changer.git
mv KDE-Terminal-Wallpaper-Changer/ksetwallpaper ~/bin
mv KDE-Terminal-Wallpaper-Changer/WallpaperChanger ~/bin
```
ksetwallpaper is the actual script that changes walls and can be used by itself. WallpaperChanger is a script example for extra automations such as time based wallpaper changing etc.

### Usage
```
ksetwallpaper $HOME/Pictures/Katie.png
ksetwallpaper $HOME/Pictures/video.mp4
ksetwallpaper $HOME/Pictures/animate.gif
```
The script now can tell what kind of file is given and use that. For this to work with videos and gifs you need the respective plugins as plasma5 does not support out of the box gifs and videos. Unfortunately the scripts that I use are the older versions from rog131 which are not available anymore, but I have put them in zip in this repo and need to be extracted in `~/.local/share/plasma/wallpapers/`.

In order to use this script for say ![Zren's inactive blur](https://store.kde.org/p/1206340/) or ![Zren's animatedhue](https://store.kde.org/p/1190533/) do:
```
ksetwallpaper "$HOME/Pictures/Katie.png" "com.github.zren.inactiveblur" "Image"
ksetwallpaper "$HOME/Pictures/Katie.png" "com.github.zren.animatedhue" "Image"
```

If you want to use this with dmenu/rofi simply `ls` or `find` the folder you want and pass it throug dmenu/rofi then use ksetwallpaper to set that wallpaper. Here is a basic example:

```
ksetwallpaper $(fd --full-path $HOME/Pictures/Wallpapers -e jpg -e jpeg -e png -e gif -e mp4 -e webm -d 2 | rofi -dmenu -i -lines 9 -p "What wallpaper?")
```
I use ![fd](https://github.com/sharkdp/fd) here but anything goes. I simply search in Pictures/Wallpapers, with a depth of 2 for any images/gif/videos you might want. I pass those through rofi and then the result is set with ksetwallpaper.

Problems: Locking the widgets makes the script not work anymore. Rog131's scripts seem to make plasmashell crash quite often.

# Lock screen
So you want to change the wallpaper from the lock screen huh?

What to do:
> make a link to a picture. Example `ln -fs ~/Pictures/Wallpapers/Image.png ~/Pictures/Wallpapers/0_Lock.png`

> then go to `Screen Locking` > Wallpaper > set `0_Lock.png` as the wallpaper.

> in order to change it simply to `ln -fs` again. Simple.

It is possible that the method above doesn't actually change your wallpaper actually, due to a config file issue. For that see the following example:

```
kwriteconfig5 --file kscreenlockerrc --group Greeter --group Wallpaper --group org.kde.image --group General --key Image "file:///home/raitaro/Pictures/Wallpapers/0_Lock.png"
```

# Login screen
Ok, so for some reason KDE Plasma 5 has to separate lock screens.

What to do for this one:
> make a link to a picture. DO: `sudo ln -fs ~/Pictures/Wallpapers/0_Lock.png /usr/share/sddm/themes/breeze/0_Lock.png`. You have to set this just once.

> now make sure this file is actually going to be used as a wallpaper ```sudo bash -c "echo 'background=0_Lock.png' >> /usr/share/sddm/themes/breeze/theme.conf"```.

> in order to change it simply do `ln -fs ~/Pictures/Wallpapers/Image.png ~/Pictures/Wallpapers/0_Lock.png` again. Here because you don't want to have to type your password every time. So you have in /usr a link that points to a link that points to an image.

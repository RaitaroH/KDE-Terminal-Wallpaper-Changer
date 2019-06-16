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
The script now can tell what kind of file is given and use that. For this to work with videos and gifs you need the respective plugins as plasma5 does not support out of the box gifs and videos. For this use the "Get new plugins" from the "Set desktop wallpaper" menu.

Problems: Do not lock your widgets.

# Lock screen
So you want to change the wallpaper from the lock screen huh?

What to do:
> make a link to a picture. Example `ln -fs ~/Pictures/Wallpapers/Image.png ~/Pictures/Wallpapers/0_Lock.png`

> then go to `Screen Locking` > Wallpaper > set `0_Lock.png` as the wallpaper.

> in order to change it simply to `ln -fs` again. Simple.

# Login screen
Ok, so for some reason KDE Plasma 5 has to separate lock screens.

What to do for this one:
> make a link to a picture. DO: `sudo ln -fs ~/Pictures/Wallpapers/0_Lock.png /usr/share/sddm/themes/breeze/0_Lock.png`. You have to set this just once.

> now make sure this file is actually going to be used as a wallpaper ```sudo bash -c "echo 'background=0_Lock.png' >> /usr/share/sddm/themes/breeze/theme.conf"```.

> in order to change it simply do `ln -fs ~/Pictures/Wallpapers/Image.png ~/Pictures/Wallpapers/0_Lock.png` again. Here because you don't want to have to type your password every time. So you have in /usr a link that points to a link that points to an image.

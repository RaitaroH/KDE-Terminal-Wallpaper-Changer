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
chmod +x ~/bin/ksetwallpaper
chmod +x ~/bin/WallpaperChanger
```
ksetwallpaper is the actual script that changes walls and can be used by itself. WallpaperChanger is a script example for extra automations such as time based wallpaper changing etc.

### Usage

#### Basic
```
ksetwallpaper $HOME/Pictures/Katie.png
ksetwallpaper $HOME/Pictures/video.mp4
ksetwallpaper $HOME/Pictures/animate.gif
```
The script now can tell what kind of file is given and use that. For this to work with videos and gifs you need the respective plugins as plasma5 does not support out of the box gifs and videos. Unfortunately the scripts that I use are the older versions from rog131 which are not available anymore, but I have put them in zip in this repo and need to be extracted in `~/.local/share/plasma/wallpapers/`.

#### Custom wallpaper plugins
In order to use this script for say ![Zren's inactive blur](https://store.kde.org/p/1206340/) or ![Zren's animatedhue](https://store.kde.org/p/1190533/) do:
```
ksetwallpaper "$HOME/Pictures/Katie.png" "com.github.zren.inactiveblur" "Image"
ksetwallpaper "$HOME/Pictures/Katie.png" "com.github.zren.animatedhue" "Image"
```

### Context menu scripts
You can move `SetAsWallpaper.desktop` and `SetAsWallpaperBlur.desktop` to `~/.local/share/kservices5/ServiceMenus/` and then you can right click on an image/video for example to set it as an wallpaper. The Blur variant only works with ![Zren's inactive blur](https://store.kde.org/p/1206340/) and only with images.

#### dmenu/rofi
If you want to use this with dmenu/rofi simply `ls` or `find` the folder you want and pass it throug dmenu/rofi then use ksetwallpaper to set that wallpaper. Here is a basic example:

```
ksetwallpaper $(fd --full-path $HOME/Pictures/Wallpapers -e jpg -e jpeg -e png -e gif -e mp4 -e webm -d 2 | rofi -dmenu -i -lines 9 -p "What wallpaper?")
```
I use ![fd](https://github.com/sharkdp/fd) here but anything goes. I simply search in Pictures/Wallpapers, with a depth of 2 for any images/gif/videos you might want. I pass those through rofi and then the result is set with ksetwallpaper.


#### pywal

For ![pywal](https://github.com/dylanaraps/pywal) you can either make some fucntion as this one:
```
wal-tile() {
	wal -n -s -t -e -i "$@"
	~/bin/ksetwallpaper "$(< "${HOME}/.cache/wal/wal")"
}
```
Or if you want to use WallpaperChanger for scripting you can:
```
wal -n -s -t -e -i "$desktop_dir$wall" > /dev/null 2>&1 # making the colors
setdesktop "$desktop_dir$wall" # calling ksetwallpaper
```
You can also extract colors to be used in say conky:

```
color1=$(cat /home/raitaro/.cache/wal/colors.sh | grep color1= | awk -F"'" '{print substr($2,2,6)}')
sed -i "/\ #modif$/s/1\ .*\ #/1\ $color1\ #/" .conky/MyConky/GothamDarkKDE
```

Where in the Conky file you have color defined as: `color1 AF5237 #modif` where the comment is needed. The code above will take the color wal generated and change it in conky so it matches your desktop.

#### Know issues

+ Locking the widgets makes the script not work anymore
+ Rog131's scripts seem to make plasmashell crash quite often

# Remove fading on wallpaper change

The time in between the wall changings is set in the files below. I have this 2 seds to do it automatically for me.

```
sudo sed -i 's/<default>1000<\/default>/<default>1<\/default>/g' /usr/share/plasma/wallpapers/org.kde.image/contents/config/main.xml
sudo sed -i 's/<default>10<\/default>/<default>1<\/default>/g' /usr/share/plasma/wallpapers/org.kde.image/contents/config/main.xml
```

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

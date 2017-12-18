# KDE-Terminal-Wallpaper-Changer
Looking around I found the code needed to change wallpapers in KDE. Unfortunately it is a pain to use in other scripts. 

The purpose? Time based wallpapers... also lock screen wallpaper changing based on time or something else.

The way I got around this was simple. I just use sed to replace `picture.png` from`"file:///home/raitaro/Pictures/Wallpapers/picture.png"`with whatever image I want specify. The gist of it is sed then you call the ksetwallpaper scrpt. That's it.

### Instalation
Simply git clone, put the scripts in your ~/bin folder and make sure you change `"file:///home/raitaro/Pictures/Wallpapers/picture.png"` at the end of ksetwallpaper with whatever directory you are using.

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
> make a link to a picture. DO: `sudo ln -fs ~/Pictures/Wallpapers/Image.png /usr/share/sddm/themes/breeze/0_Lock.png`. You have to set this just once.
> then go to `Login Screen (SDDM)` > Background > set `0_Lock.png` as the wallpaper.
> in order to change it simply do `ln -fs ~/Pictures/Wallpapers/Image.png ~/Pictures/Wallpapers/0_Lock.png` again. Here because you don't want to have to type your password every time. So you have in /usr a link that points to a link that points to an image.

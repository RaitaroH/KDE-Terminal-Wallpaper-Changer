#!/usr/bin/env bash

full_image_path=$(realpath "$1")
ext=$(file -b --mime-type "$full_image_path")

if [ -z "$2" ]; then
    # Identify filetype and make changes
    case $(echo $ext | cut -d'/' -f2) in
        "mp4"|"webm") type='VideoWallpaper' ; write='VideoWallpaperBackgroundVideo';;
        "png"|"jpeg") type='org.kde.image' ; write='Image' ;;
        "gif"|"webp") type='GifWallpaper' ; write="GifWallpaperBackgroundGif" ;;
    esac
else
    type="$2";
    write="$3";
fi

wallpaper_set_script="var allDesktops = desktops();
    print (allDesktops);
    for (i=0;i<allDesktops.length;i++)
    {
        d = allDesktops[i];
        d.wallpaperPlugin = '${type}';
        d.currentConfigGroup = Array('Wallpaper', '${type}', 'General');
        d.writeConfig('Image', 'file:///dev/null')
        d.writeConfig('$write', 'file://${full_image_path}')
    }"

qdbus org.kde.plasmashell /PlasmaShell org.kde.PlasmaShell.evaluateScript "${wallpaper_set_script}"

# Optional, change lockscreen if you want
# kwriteconfig5 --file kscreenlockerrc --group Greeter --group Wallpaper --group org.kde.image --group General --key Image "file://$full_image_path"

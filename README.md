Minimal Packages:

```
wayland wayland-protocols wlroots_0_19 base-devel git rofi kitty wl-clipboard grim slurp swaybg thunar mako xorg-xwayland otf-departure-mono-nerd
```
changes you can do: 

browser of choice, browser isnt specified since we xdg-open http:// so it just opens whatever your default is

we use mako as swaync for some reason breaks after opening it a single time no matter what I have tried

modify config.def.h to change your font, file manager, app launcher. and terminal if you want to

-------------

What I am using from my dotfiles:

```
stow -R kanshi kitty mako mpv nvim rofi scripts shell yazi zsh
```

sudo make clean install in both dwl and slstatus

id suggest after installing to make your own start_dwl script to launch whatever you need similar to this, and make it so your login manager like ly or sddm can use it


```
#!/bin/sh

export XDG_CURRENT_DESKTOP=DWL
export XDG_SESSION_DESKTOP=DWL
export XDG_SESSION_TYPE=wayland

export QT_QPA_PLATFORM=wayland
export QT_QPA_PLATFORMTHEME=qt6ct
export QT_WAYLAND_DISABLE_WINDOWDECORATION=1
export QT_AUTO_SCREEN_SCALE_FACTOR=1

export ELECTRON_OZONE_PLATFORM_HINT=auto
export LIBVA_DRIVER_NAME=nvidia
export __GLX_VENDOR_LIBRARY_NAME=nvidia
export NVD_BACKEND=direct
export GSK_RENDERER=ngl
export MOZ_ENABLE_WAYLAND=1
export MOZ_DISABLE_RDD_SANDBOX=1
export EGL_PLATFORM=wayland
export CLUTTER_BACKEND=wayland
export GDK_BACKEND=wayland,x11,*
export GDK_SCALE=1

exec sh -c '
slstatus -s | dwl -s "sh -c \"
    ~/.config/dwl/autostart/set-res.sh &
    easyeffects --gapplication-service &
    wl-paste --type image --watch cliphist store &
    wl-paste --type text --watch cliphist store &
    mako &
	easyeffects &
	kanshi &
	(sleep 5 && trash-empty 30 -f) &
    swaybg -m fill -i \\\"$(cat ~/.config/hypr/wallpaper)\\\"
\""
'
```

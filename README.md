to be used with my dotfiles, to get scripts. you really dont even need that much.

wayland wayland-protocols wlroots_0_19 base-devel git rofi kitty wl-clipboard swaybg thunar xorg-xwayland 

font of choice, browser of choice, browser isnt specified since we xdg-open http:// so it just opens whatever your default is.

filemanager of choice but thunar is default

modify config.def.h to change your font, and file manager if you choose to.

just remember to sudo make clean install in dwl and slstatus.


id suggest after installing to make your own launcher for dwl to launch whatever you need similar to this, this allows you to use wayland-logout to just cleanly quit dwl (or whatever powermenu script you want), if you choose not to there is still CTRL ALT Backspace to exit out of the session.

session-apps.service is swaync and kanshi, and easyeffects again since sometimes the --gapplication-service does not apply the eq but running it again separately without --gapplication-service does. really odd to me, but whatever.


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
    slstatus -s | dwl &
    DWL_PID=$!

    while [ ! -S "$XDG_RUNTIME_DIR/wayland-0" ]; do
        sleep 0.2
    done

    ~/.config/dwl/autostart/set-res.sh

    easyeffects --gapplication-service &

    swaybg -m fill -i "$(cat ~/.config/hypr/wallpaper)" 

    pgrep -x wl-paste >/dev/null || {
        wl-paste --type image --watch cliphist store &
        wl-paste --type text --watch cliphist store &
    }

    (sleep 5 && trash-empty 30 -f) &

    systemctl --user start session-apps.service &

    wait "$DWL_PID"
'

```

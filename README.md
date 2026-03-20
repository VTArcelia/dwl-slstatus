to be used with my dotfiles, to get scripts. you really dont even need that much.

wayland wayland-protocols wlroots_0_19 base-devel git rofi kitty wl-clipboard swaybg thunar xorg-xwayland 

optional: xwayland-satellite

font of choice, browser of choice, browser isnt specified since we xdg-open http:// so it just opens whatever your default is.

filemanager of choice but thunar is default

modify config.def.h to change your font, and file manager if you choose to.

no idea why my screenshots are not working rn so no grim / slurp

just remember to sudo make clean install in dwl and slstatus.


id suggest after installing to make your own launcher for dwl to launch whatever you need similar to this, this allows you to use wayland-logout to just cleanly quit dwl, if you choose not to there is still the standard SUPER SHIFT Q to exit out of the session.



```
#!/bin/sh

xwayland-satellite &

exec sh -c '
    slstatus -s | dwl &
    DWL_PID=$!

    while [ ! -S "$XDG_RUNTIME_DIR/wayland-0" ]; do
        sleep 0.1
    done

    swaybg -i /home/arcy/walls/daily/1zKBVcp.png &

    ~/.config/dwl/autostart/set-res.sh

    easyeffects --gapplication-service &
    swaync &

    pgrep -x wl-paste >/dev/null || {
        wl-paste --type image --watch cliphist store &
        wl-paste --type text --watch cliphist store &
    }

    (sleep 5 && trash-empty 30 -f) &

    wait "$DWL_PID"
'
```

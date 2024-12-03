# Hyprpaper
`~/.config/hypr/hyprpaper.conf`
```
preload = ~/.config/hypr/wallpaper/whiskey8_upscale_crop.png
wallpaper = DP-1,~/.config/hypr/wallpaper/whiskey8_upscale_crop.png
```

# Hyprlock
`~/.config/hypr/hyprlock.conf`
```
background{
    monitor = 
    path = /etc/nixos/summer-night.png
    #path = screenshot
    color = rgba(25, 20, 20, 1.0)
    blur_passes = 2
    blur_size = 2
}

label {
    text = cmd[update:1000] echo "$(date +"%H:%M:%S")"
    #color = $foreground
    color = rgba(255, 255, 255, 0.6)
    font_size = 120
    font_family = MesloLGS Nerd Font Mono Bold
    position = 0, -300
    halign = center
    valign = top
}

label {
    text = Hi there, $USER
    #color = $foreground
    color = rgba(255, 255, 255, 0.6)
    font_size = 25
    font_family = MesloLGS Nerd Font Mono
    position = 0, -40
    halign = center
    valign = center
}

input-field {
    size = 250, 60
    outline_thickness = 2
    dots_size = 0.2 # Scale of input-field height, 0.2 - 0.8
    dots_spacing = 0.2 # Scale of dots' absolute size, 0.0 - 1.0
    dots_center = true
    outer_color = rgba(0, 0, 0, 0)
    inner_color = rgba(0, 0, 0, 0.5)
    font_color = rgb(200, 200, 200)
    fade_on_empty = false
    #font_family = MesloLGS Nerd Font Mono
    placeholder_text = <i><span foreground="##cdd6f4">Password</span></i>
    hide_input = false
    position = 0, -120
    halign = center
    valign = center
}
```

# Hypridle
`~/.config/hypr/hypridle.conf`
```
general {
    lock_cmd = pidof hyprlock || hyprlock
    before_sleep_cmd = loginctl lock-session
    after_sleep_cmd = hyprctl dispatch dpms on
}

listener {
    timeout = 290
    on-timeout = systemctl --user stop auto-brightness.timer && sleep 10 && brightnessctl -s set 0
    on-resume = systemctl --user start auto-brightness.timer && brightnessctl -r
}

listener {
    timeout = 600
    on-timeout = loginctl lock-session
}

listener {
    timeout = 630
    on-timeout = hyprctl dispatch dpms off
    on-resume = hyprctl dispatch dpms on
}

listener {
    timeout = 900
    on-timeout = sh ~/.config/scripts/frontlight.sh off && systemctl --user start auto-brightness.timer && systemctl suspend
}
```
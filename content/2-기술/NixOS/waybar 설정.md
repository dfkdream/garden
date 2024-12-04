![[Pasted image 20241204102742.png]]
# config
`~/.config/waybar/config`
```
{
    "layer": "top", // Waybar at top layer
    "position": "top", // Waybar position (top|bottom|left|right)
    "height": 30, // Waybar height (to be removed for auto height)
    "spacing": 4, // Gaps between modules (4px)
	"margin": "10 10 0 10",

    // Choose the order of the modules
    "modules-left": ["hyprland/workspaces"],
    "modules-center": ["hyprland/window"],
    "modules-right": ["tray", "idle_inhibitor", "pulseaudio", "backlight", "network", "group/system", "clock"],

    // Modules configuration
    "hyprland/workspaces": {
		"on-scroll-up": "hyprctl dispatch workspace e+1",
		"on-scroll-down": "hyprctl dispatch workspace e-1"
    },
    "idle_inhibitor": {
        "format": "{icon}",
        "format-icons": {
            "activated": "󰒳 ",
            "deactivated": "󰒲 "
        }
    },
    "tray": {
        // "icon-size": 21,
        "spacing": 10
    },
    "clock": {
		"interval": 1,
		"format": "{:%Y-%m-%d %H:%M}",
        "format-alt": "{:%Y-%m-%d %H:%M:%S}",
        "tooltip-format": "<tt>{calendar}</tt>",
		"calendar": {
			"format": {
				"months": "{}\n",
				"weekdays": "Su Mo Tu We Th Fr Sa",
				"today": "<span color='#ffcc66'><b>{}</b></span>"
			}
		}
    },
	"cpu": {
		"format": " {usage}%",
		"tooltip": false,
		"on-click": "gnome-system-monitor"
	},
	"memory": {
		"format": " {}%"
	},
	"temperature": {
		"hwmon-path": "/sys/class/hwmon/hwmon1/temp1_input",
		"critical-threshold": 80,
		"format": "{icon} {temperatureC}°C",
		"format-icons": ["", "", ""]
	},
	"group/system": {
		"orientation": "horizontal",
		"drawer": {
			"transition-left-to-right": false,
			"transition-duration": 100
		},
		"modules":["cpu", "temperature", "memory"]
	},
    "backlight": {
		"scroll-step": 10,
        "format": "{icon}{percent}%",
        "format-icons": [" ", " ", " ", " ", " ", " ", " ", " ", " "],
		"on-click": "sh ~/.config/scripts/backlight-mode.sh"
    },
    "network": {
        "format-wifi": "󰖩 {signalStrength}%",
        "format-ethernet": " {ipaddr}/{cidr}",
        "tooltip-format": " {ifname} via {gwaddr}",
        "format-linked": " {ifname} (No IP)",
        "format-disconnected": "⚠ Disconnected",
        "format-alt": "{ifname}: {ipaddr}/{cidr}"
    },
    "pulseaudio": {
        "format": "{icon}{volume}% {format_source}",
        "format-bluetooth": "{icon}{volume}% {format_source}",
        "format-bluetooth-muted": "{icon}  {format_source}",
        "format-muted": "  {format_source}",
		"format-source": " {volume}%",
        "format-source-muted": " ",
        "format-icons": {
            "headphone": " ",
            "hands-free": "󰋎 ",
            "headset": "󰋎 ",
            "phone": " ",
            "portable": " ",
            "car": " ",
            "default": [" ", " ", " "]
        },
        "on-click": "sh ~/.config/scripts/toggle-sink.sh",
        "on-double-click": "pavucontrol"
    }
}
```
# style.css
`~/.config/waybar/style.css`
```css
* {
    font-family: monospace;
    font-size: 15px;
}

window#waybar {
    background-color: rgba(92, 106, 114, 0.5);
    border-bottom: 3px solid rgba(100, 114, 125, 0.5);
    color: #fdf6e3;
    transition-property: background-color;
    transition-duration: .5s;
	border-radius: 10px;
}

window#waybar.hidden {
    opacity: 0.2;
}

.module {
	border-bottom: 3px solid rgba(0, 0, 0, 0.2);
}

button {
	border: none;
	border-radius: 0;
}

button:hover {
    background: inherit;
	text-shadow: inherit;
	border: none;
	box-shadow: none;
}

#workspaces button {
	margin: 0;
    padding: 0 5px;
    background-color: transparent;
    color: #ffffff;
}

#workspaces button:hover {
    background: rgba(0, 0, 0, 0.2);
}

#workspaces button.active {
    background: rgba(0, 0, 0, 0.2);
    background-color: #64727D;
}

#workspaces button.urgent {
    background-color: #eb4d4b;
}

#mode {
    background-color: #64727D;
    border-bottom: 3px solid #ffffff;
}

#workspaces,
#clock,
#cpu,
#memory,
#disk,
#temperature,
#backlight,
#network,
#pulseaudio,
#wireplumber,
#tray,
#idle_inhibitor,
#mpd {
	border-radius: 10px;
    padding: 0 13px;
    color: #fdf6e3;
}

#window,
#workspaces {
    margin: 0 4px;
}

/* If workspaces is the leftmost module, omit left margin */
.modules-left > widget:first-child > #workspaces {
    margin-left: 0;
}

/* If workspaces is the rightmost module, omit right margin */
.modules-right > widget:last-child > #workspaces {
    margin-right: 0;
}

#workspaces {
	background-color: #a6b0a0;
}

#clock {
    background-color: #64727D;
}

label:focus {
    background-color: #000000;
}

#cpu {
    background-color: #2ecc71;
    color: #000000;
}

#memory {
	margin: 0 4px;
    background-color: #9b59b6;
}

#disk {
    background-color: #964B00;
}

#backlight {
    background-color: #90b1b1;
	/*padding-right: 10px;*/
}

#network {
    background-color: #2980b9;
}

#network.disconnected {
    background-color: #f53c3c;
}

#pulseaudio {
    background-color: #f1c40f;
    color: #000000;
}

#pulseaudio.muted {
    background-color: #90b1b1;
    color: #2a5c45;
}

#wireplumber {
    background-color: #fff0f5;
    color: #000000;
}

#wireplumber.muted {
    background-color: #f53c3c;
}

#temperature {
    background-color: #f0932b;
}

#temperature.critical {
    background-color: #eb4d4b;
}

#tray {
    background-color: #2980b9;
}

#tray > .passive {
    -gtk-icon-effect: dim;
}

#tray > .needs-attention {
    -gtk-icon-effect: highlight;
    background-color: #eb4d4b;
}

#idle_inhibitor {
    background-color: #2d3436;
	padding-right: 10px;
}

#idle_inhibitor.activated {
    background-color: #ecf0f1;
    color: #2d3436;
}

#mpd {
    background-color: #66cc99;
    color: #2a5c45;
}

#mpd.disconnected {
    background-color: #f53c3c;
}

#mpd.stopped {
    background-color: #90b1b1;
}

#mpd.paused {
    background-color: #51a37a;
}
```
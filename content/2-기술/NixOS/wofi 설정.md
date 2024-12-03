![[Pasted image 20241203232638.png]]
# config
`~/.config/wofi/config`
```
mode=drun
allow_images=true
image_size=20
term=wezterm
insensitive=true
location=center
no_actions=true
prompt=Search
width=30%
```
# style.css
`~/.config/wofi/style.css`
```css
* {
    font-family: "MesloLGS Nerd Font", FontAwesome;
    font-size: 15px;
    border-radius: 10px;
    border: none;
	outline: none;
}

window {
    margin: 0px;
    background-color: #5c6a72;
    color: #fdf6e3;

	border-radius: 10px;

    border-bottom-width: 5px;
	border-bottom-color: rgba(0,0,0,0.3);
    border-bottom-style: solid;
}

#outer-box {
    margin: 0px;
    border-radius: 0px;
}

#input {
    background-color: #acc404;
    color: #fdf6e3;

    margin: 10px;
    padding: 5px 10px;
    border: none;

    border-bottom-width: 5px;
	border-bottom-color: rgba(0,0,0,0.3);
    border-bottom-style: solid;
}

#inner-box {
    margin: 0px 10px;
    padding: 0px;
    border-radius: 0px;
}

#scroll {
    margin: 0px;
	margin-bottom: 10px;
    padding: 0px;
    border: none;
}

#text {
    margin-left: 15px;
    margin-right: 15px;
}

#entry {
    border: none;
    padding: 10px;
    margin: 0px;
}

#entry:selected {
    background-color: #fdf6e3;

	border-style: none;
    border-bottom-width: 5px;
	border-bottom-color: rgba(0,0,0,0.3);
    border-bottom-style: solid;
}

#entry:selected #text {
	color: #5c6a72;
}
```
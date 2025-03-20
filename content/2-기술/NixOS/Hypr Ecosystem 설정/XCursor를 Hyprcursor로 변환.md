[Notwaita Black](https://github.com/ful1e5/notwaita-cursor) 폰트를 기준으로 작성했다.
## Hyprcursor 설치
```
nix shell nixpkgs#hyprcursor nixpkgs#xcur2png
```
## XCursor 패키지 추출
```zsh
hyprcursor-util --extract ./Notwaita-Black
```

현재 위치에 `extracted_Notwaita-Black` 폴더가 생성된다.
폴더 루트에 있는 `manifest.hl` 파일을 확인하고 원하는 대로 수정한다. 내 경우 폰트 이름이 제대로 지정되어 있지 않아 수정해야 했다.
## Hyprcursor 패키지 생성
```zsh
hyprcursor-util --create ./extracted_Notwaita-Black
```

현재 위치에 `theme_Notwaita-Black-hyprcursor` 폴더가 생성된다. 폴더 이름은 `manifest.hl`에서 지정한 이름에 따라 달라진다.
## Hyprcursor 패키지 설치
```zsh
cp -r theme_Notwaita-Black-hyprcursor ~/.local/share/icons/Notwaita-Black
```
## 테스트
```zsh
hyprctl setcursor Notwaita-Black 24
```
## 설정
`~/.config/hypr/hyprland.conf`에 다음 설정을 추가한다.
```
env = HYPRCURSOR_THEME,Notwaita-Black
env = HYPRCURSOR_SIZE,24
```
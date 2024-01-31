그냥 패키지 매니저만 쓸까
# Live CD 부팅
최상단 기본 옵션으로 부팅하면 됨

nomodeset: 그래픽 가속 없이 부팅 https://askubuntu.com/questions/207175/what-does-nomodeset-do
# 루트 전환
```bash
sudo -i
```
# 디스크 파티셔닝 (GPT)
```bash
parted /dev/vda -- mklabel gpt
parted /dev/vda -- mkpart root 512MB -8GB #vda1
parted /dev/vda -- mkpart swap -8GB 100% #vda2
parted /dev/vda -- mkpart ESP fat32 1MB 512MB #vda3
parted /dev/vda -- set 3 esp on
```
# 디스크 포맷
```bash
mkfs.ext4 -L nixos /dev/vda1
mkswap -L swap /dev/vda2
mkfs.fat -F 32 -n boot /dev/vda3
```
# 설치
## 마운트
```bash
mount /dev/disk/by-label/nixos /mnt
mkdir -p /mnt/boot
mount /dev/disk/by-label/boot /mnt/boot
```
## 설정 파일 생성
```bash
nixos-generate-config --root /mnt
```
`/mnt/etc/nixos/configuration.nix`에 생성됨
## 설치
```bash
nixos-install
reboot
```

# FDE
https://nixos.wiki/wiki/Full_Disk_Encryption
디스크 파티셔닝 이후
```bash
cryptsetup luksFormat /dev/vda1
cryptsetup luksOpen /dev/vda1 cryptroot
mkfs.ext4 -L nixos /dev/mapper/cryptroot

mount /dev/disk/by-label/nixos /mnt
```
이후로 동일하게 진행

## Plymouth 복호화 화면 띄우기
```nix
boot.initrd.enable = true;
boot.plymouth.enable = true;
# 선택
# boot.kernelParams = ["quiet"];
```
# Hyprland 설정
![[Pasted image 20231215165700.png]]
## configuration.nix
```nix
xdg.portal.enable = true;
xdg.portal.extraPortals = [ pkgs.xdg-desktop-portal-gtk ];

programs.hyprland = {
	enable = true;
	xwayland.enable = true;
};

environment.sessionVariables = {
	WLR_NO_HARDWARE_CURSORS = "1";
	NIXOS_OZONE_WL = "1";
}

hardware.opengl.enable = true;

environment.systemPackages = with pkgs; [
	kitty
	wofi
	waybar
	# ...
];

fonts.packages = withpkgs; [
	font-awesome #for waybar
	# ...
]
```
## hyprland.conf
```
# ...
exec-once = waybar
# ...
```
## waybar
https://github.com/Alexays/Waybar/wiki/Examples

https://github.com/sephid86/archas/tree/master/skel/.config/waybar

`~/.config/waybar`에 `style.css`, `config` 파일 붙여넣기

sway 기준으로 작성되어 있어 수정 필요
* `config` 파일 `sway/*`를 모두 `hyprland/*`로 변경
* `style.css` 파일 `#workspace button.focused`를 `#workspace button.active`로 변경 (워크스페이스가 안 보이는 경우 - 테스트 안해봄)
* https://wiki.hyprland.org/Useful-Utilities/Status-Bars/

## kitty.conf
```
background_opacity 0.5
```
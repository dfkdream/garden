# flake.nix
`/etc/nixos/flake.nix`
```nix
{
  inputs = {
    nixpkgs.url = "github:NixOS/nixpkgs/nixos-unstable";
    nixpkgs-kime.url = "github:NixOS/nixpkgs/a3c0b3b21515f74fd2665903d4ce6bc4dc81c77c";

    sddm-sugar-candy.url = "gitlab:Zhaith-Izaliel/sddm-sugar-candy-nix/35d6fdb4eed20d682e992ae3c6095427e89c2b74";

    auto-brightness.url = "github:dfkdream/auto-brightness";
  };

  outputs = { self, nixpkgs, ... }@inputs: 
  let
    overlays-module = { config, pkgs, ... }: {
      nixpkgs.overlays = [
	(final: prev: {
	  auto-brightness = inputs.auto-brightness.packages.${prev.system}.default;
	  kime = inputs.nixpkgs-kime.legacyPackages.${prev.system}.kime;
	})
      ];
    };
  in
  {
    nixosConfigurations.nixos = nixpkgs.lib.nixosSystem{
      system = "x86_64-linux";
      specialArgs = { inherit inputs; };
      modules = [
        ./configuration.nix

	overlays-module
	inputs.sddm-sugar-candy.nixosModules.default
      ];
    };
  };
}
```

# configuration.nix
수정해야 할 부분들:
* `users` 부분 사용자 이름 수정
* `udev` 부분 하드웨어 ID 수정

`/etc/nixos/configuration.nix`
```nix
# Edit this configuration file to define what should be installed on
# your system.  Help is available in the configuration.nix(5) man page
# and in the NixOS manual (accessible by running ‘nixos-help’).

{ config, pkgs, inputs, lib, ... }:
{
  imports =
    [ # Include the results of the hardware scan.
      ./hardware-configuration.nix
    ];

  nix.settings.experimental-features = [ "nix-command" "flakes" ];

  # Bootloader.
  boot.loader.systemd-boot.enable = true;
  boot.loader.efi.canTouchEfiVariables = true;

  # Plymouth for luks decryption screen
  #boot.initrd.systemd.enable = true;
  #boot.plymouth.enable = true;

  # To temporarily resolve faulty USB
  boot.kernelParams = [
    "usbcore.initial_descriptor_timeout=100"
  ];

  boot.extraModulePackages = with config.boot.kernelPackages; [ ddcci-driver ];
  boot.kernelModules = [ "ddcci" ];

  services.logind.extraConfig = ''
    HandlePowerKey=ignore
  '';

  hardware.bluetooth.enable = true;
  hardware.bluetooth.powerOnBoot = true;

  networking.hostName = "nixos"; # Define your hostname.
  # networking.wireless.enable = true;  # Enables wireless support via wpa_supplicant.

  # Configure network proxy if necessary
  # networking.proxy.default = "http://user:password@proxy:port/";
  # networking.proxy.noProxy = "127.0.0.1,localhost,internal.domain";

  # Enable networking
  networking.networkmanager.enable = true;

  # Set your time zone.
  time.timeZone = "Asia/Seoul";

  # Select internationalisation properties.
  i18n = { 
    defaultLocale = "ko_KR.UTF-8";

    extraLocaleSettings = {
      LC_ADDRESS = "ko_KR.UTF-8";
      LC_IDENTIFICATION = "ko_KR.UTF-8";
      LC_MEASUREMENT = "ko_KR.UTF-8";
      LC_MONETARY = "ko_KR.UTF-8";
      LC_NAME = "ko_KR.UTF-8";
      LC_NUMERIC = "ko_KR.UTF-8";
      LC_PAPER = "ko_KR.UTF-8";
      LC_TELEPHONE = "ko_KR.UTF-8";
      LC_TIME = "ko_KR.UTF-8";
    };

    inputMethod.type = "kime";
    inputMethod.enable = true;
  };

  fonts = {
    packages = with pkgs; [
      nanum
      (nerdfonts.override { fonts = [ "Meslo" ]; })
      font-awesome
      noto-fonts
      noto-fonts-cjk-sans
      noto-fonts-cjk-serif
      noto-fonts-emoji
      source-han-sans
      source-han-serif
    ];

    fontconfig.defaultFonts = {
      serif = ["NanumMyeongjo"];
      sansSerif = ["NanumGothic"];
    };
  };

  # Enable the X11 windowing system.
  services.xserver.enable = true;

  xdg.portal = {
    enable = true;
    extraPortals = with pkgs; [
      #xdg-desktop-portal-hyprland
      xdg-desktop-portal-wlr
    ];
  };

  programs.hyprland = {
    enable = true;
    xwayland.enable = true;
  };

  environment.sessionVariables = {
    WLR_NO_HARDWARE_CURSORS = "1";
    #NIXOS_OZONE_WL = "1";
  };

  hardware.graphics.enable = true;

  # Enable the GNOME Desktop Environment.
  services.displayManager = {
    #gdm.enable = true;
    sddm.extraPackages = [ pkgs.libsForQt5.qt5.qtgraphicaleffects ];
    sddm.enable = true;
    defaultSession = "hyprland";
    sddm.theme = "sddm-sugar-candy-nix";
    sddm.settings = {
      General = {
        GreeterEnvironment = "QT_SCREEN_SCALE_FACTORS=2";
      };
    };
    sddm.sugarCandyNix = {
      enable = true;
      settings = {
        #FormPosition = "left";
		Background = lib.cleanSource ./summer-night.png;
		PartialBlur = true;
		#MainColor = "#333";
		#DimBackgroundImage = 0.5;
		ForceHideCompletePassword = true;
      };
    };
  };

  services.xserver.desktopManager.gnome.enable = true;

  # Configure keymap in X11
  services.xserver = {
    xkb.layout = "kr";
    xkb.variant = "kr104";
  };

  # Enable CUPS to print documents.
  services.printing = {
    enable = true;
    drivers = with pkgs; [ cnijfilter2 ];
    browsed.enable = false;
  };

  # Enable sound with pipewire.
  # sound.enable = true;
  hardware.pulseaudio.enable = false;
  security.rtkit.enable = true;
  services.pipewire = {
    enable = true;
    alsa.enable = true;
    alsa.support32Bit = true;
    pulse.enable = true;
    # If you want to use JACK applications, uncomment this
    #jack.enable = true;

    # use the example session manager (no others are packaged yet so this is enabled by default,
    # no need to redefine it in your config for now)
    #media-session.enable = true;
  };

  services.avahi = {
    enable = true;
    nssmdns4 = true;
    openFirewall = true;
  };

  # Enable touchpad support (enabled default in most desktopManager).
  # services.xserver.libinput.enable = true;

  # Define a user account. Don't forget to set a password with ‘passwd’.
  users.users.dfkdream = {
    isNormalUser = true;
    description = "dfkdream";
    extraGroups = [ "networkmanager" "wheel" "wireshark" "dialout" "video" ];
    packages = with pkgs; [
      firefox
      kitty
      nextcloud-client
      obsidian
      discord
      obs-studio
      cider
      go
      nodejs
      grim
      slurp
      auto-brightness
      clang-tools
      openscad
    ];
  };

  # Allow unfree packages
  nixpkgs.config.allowUnfree = true;

  nixpkgs.config.permittedInsecurePackages = [
    "electron-25.9.0"
  ];

  # List packages installed in system profile. To search, run:
  # $ nix search wget
  environment.systemPackages = with pkgs; [
    neovim # Do not forget to add an editor to edit configuration.nix! The Nano editor is also installed by default.
    wget
    rofi-wayland
    wofi
    waybar
    dunst
    libnotify
    git
    fzf
    pavucontrol
    pulseaudio
    nwg-look
    hyprlock
    hyprpaper
    hypridle
    wl-clipboard
    brightnessctl
    blueberry
    screen
    usbutils
    vscode
    #pinentry-curses
  ];


  programs.zsh.enable = true;
  users.defaultUserShell = pkgs.zsh;

  programs.steam.enable = true;

  programs.wireshark.enable = true;
  programs.wireshark.package = pkgs.wireshark;

  # Some programs need SUID wrappers, can be configured further or are
  # started in user sessions.
  # programs.mtr.enable = true;
  services.pcscd.enable = true;
  programs.gnupg.agent = {
    enable = true;
    pinentryPackage = pkgs.pinentry-gnome3;
    #pinentryFlavor = "curses";
    enableSSHSupport = true;
  };

  # List services that you want to enable:

  # Enable the OpenSSH daemon.
  # services.openssh.enable = true;

  # Open ports in the firewall.
  networking.firewall.allowedTCPPorts = [ 5173 8080 ];
  networking.firewall.allowedUDPPorts = [ 6002 ];
  # Or disable the firewall altogether.
  #networking.firewall.enable = false;

  services.udev.extraRules = ''
    ACTION=="add", SUBSYSTEM=="backlight", RUN+="${pkgs.coreutils}/bin/chgrp video /sys/class/backlight/%k/brightness"
    ACTION=="add", SUBSYSTEM=="backlight", RUN+="${pkgs.coreutils}/bin/chmod g+w /sys/class/backlight/%k/brightness"
    ACTION=="add", SUBSYSTEM=="i2c", ATTR{name}=="AMDGPU DM aux hw bus*", TAG+="ddcci", TAG+="systemd", ENV{SYSTEMD_WANTS}+="ddcci@$kernel.service"
  '';

  systemd.services."ddcci@" = {
    scriptArgs = "%i";
    script = ''
      echo Trying to attach ddcci to $1
      echo ddcci 0x37 > /sys/bus/i2c/devices/$1/new_device
      exit $?
    '';
    serviceConfig.Type = "oneshot";
  };

  systemd.services."afterSuspend" = {
    enable = true;
    description = "Run these after suspend";
    serviceConfig = {
      User = "dfkdream";
      Type = "oneshot";
    };
    wantedBy = ["sleep.target" "network-online.target"];
    after = ["systemd-suspend.service"];
    script = ''
      /bin/sh /home/dfkdream/.config/scripts/frontlight.sh on
    '';
  };

  services.udev.extraHwdb = ''
    evdev:input:b0003v045Ep07A5*
     KEYBOARD_KEY_700e7=forward
  '';

  # This value determines the NixOS release from which the default
  # settings for stateful data, like file locations and database versions
  # on your system were taken. It‘s perfectly fine and recommended to leave
  # this value at the release version of the first install of this system.
  # Before changing this value read the documentation for this option
  # (e.g. man configuration.nix or on https://nixos.org/nixos/options.html).
  system.stateVersion = "23.11"; # Did you read the comment?

}
```
# kime
> [!warning]
> `xim_preedit_font`가 설치되어 있지 않으면 오류가 발생한다. 디버그하기 어려우니 주의...

`~/.config/kime/config.yaml`
```yaml
daemon:
  modules:
    - Xim
    - Wayland
    - Indicator

indicator:
  icon_color: White

log:
  global_level: DEBUG

engine:
  default_category: Latin
  global_category_state: false
  global_hotkeys:
    Super-C-Backslash:
      behavior: !Mode Math
      result: ConsumeIfProcessed

    C-Space:
      behavior: !Toggle
        - Hangul
        - Latin
      result: Consume

    S-Space:
      behavior: !Toggle
        - Hangul
        - Latin
      result: Consume

    Super-C-Space:
      behavior: !Mode Emoji
      result: ConsumeIfProcessed

    Esc:
      behavior: !Switch Latin
      result: Bypass

    Muhenkan:
      behavior: !Toggle
        - Hangul
        - Latin
      result: Consume

    Hangul:
      behavior: !Toggle
        - Hangul
        - Latin
      result: Consume

  category_hotkeys:
    Hangul:
      F9:
        behavior: !Mode Hanja
        result: Consume

      HangulHanja:
        behavior: !Mode Hanja
        result: Consume

  mode_hotkeys:
    Math:
      Enter:
        behavior: Commit
        result: ConsumeIfProcessed

      Tab:
        behavior: Commit
        result: ConsumeIfProcessed

      Esc:
        behavior: Commit
        result: Bypass

    Hanja:
      Enter:
        behavior: Commit
        result: ConsumeIfProcessed

      Tab:
        behavior: Commit
        result: ConsumeIfProcessed

    Emoji:
      Enter:
        behavior: Commit
        result: ConsumeIfProcessed

      Tab:
        behavior: Commit
        result: ConsumeIfProcessed

      Esc:
        behavior: Commit
        result: Bypass

  xim_preedit_font:
    - Noto Sans Mono CJK KR
    - 15.0

  latin:
    layout: Qwerty
    preferred_direct: true

  hangul:
    layout: dubeolsik
    word_commit: false
    preedit_johab: Needed
    addons:
      all:
        - ComposeChoseongSsang
        - DecomposeChoseongSsang
        - ComposeJungseongSsang
        - FlexibleComposeOrder
      dubeolsik:
        - TreatJongseongAsChoseong
```
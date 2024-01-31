# UserChrome.css
```css
/* 타이틀 바에서 탭 목록 숨기기
 * 창 제목은 도구 모음 사용자 지정 -> 하단 제목 표시줄 체크로 활성화 */
#main-window[tabsintitlebar="true"]:not([extradragspace="true"]) #TabsToolbar > .toolbar-items {
  opacity: 0;
  pointer-events: none;
}
#main-window:not([tabsintitlebar="true"]) #TabsToolbar {
    visibility: collapse !important;
}

/* 사이드 바에서 트리 스타일 탭 제목 숨기기 */
#sidebar-box[sidebarcommand="treestyletab_piro_sakura_ne_jp-sidebar-action"] #sidebar-header {
  display: none;
}
```

`about:config`에서 `toolkit.legacyUserProfileCustomizations.styleshe` 설정을 `true`로 변경해야 적용된다.
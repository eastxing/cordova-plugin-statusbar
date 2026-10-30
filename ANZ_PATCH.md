# 3.0.0-anz.1

Fork of Apache [`cordova-plugin-statusbar` 3.0.0](https://github.com/apache/cordova-plugin-statusbar/tree/3.0.0).

- **Repo:** https://github.com/eastxing/cordova-plugin-statusbar
- **Branch:** `anz-3.0.0`
- **Tag:** `3.0.0-anz.1`
- **Plugin id:** `cordova-plugin-statusbar` (unchanged so Cordova wiring stays compatible)

`master` on this GitHub fork tracks Apache (currently 4.x). **Do not install `master`.** Pin the tag.

## Why

Upstream `resizeWebView` uses `UIApplication.statusBarFrame`. On iOS 17+ / 27 that
height is often **0**, so `overlaysWebView(false)` does not inset the WebView.
Upstream **4.0.0** does not fix this.

## iOS changes (`src/ios/CDVStatusBar.m`)

1. **`-anz_statusBarFrame`** — `UIStatusBarManager` → legacy `statusBarFrame` →
   max with `safeAreaInsets.top`. Used for WebView inset and the native strip.

2. **`-anz_presentStatusBarBackgroundView`** — keep the theme-colored strip
   sized and in front after overlays/color/resize; paint `viewController.view`
   so a late WebView inset does not show white behind the status bar.

3. **Logs** (`[ANZ-SB]`): `overlays a→b`, `bgColor #RRGGBB`, `_ready RACE` only.

## App contract

- Default / pref `StatusBarOverlaysWebView` stays **true**.
- App JS must call `StatusBar.overlaysWebView(false)` to trigger native resize.

## Install

```bash
cordova plugin add https://github.com/eastxing/cordova-plugin-statusbar.git#3.0.0-anz.1
```

## Next ANZ release

Bump `package.json` + `plugin.xml` to `3.0.0-anz.2` (or later), commit on
`anz-3.0.0`, tag, and point the app at the new tag.

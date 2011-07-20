---
layout: product2
title: TotalTerminal is a system-wide terminal accessible via a hot-key
product: totalterminal
product_title: TotalTerminal
product_subtitle: a system-wide terminal on a hot-key
note: If you like TotalTerminal, check out also <a href="http://totalfinder.binaryage.com">TotalFinder</a>.
download: http://downloads.binaryage.com/TotalTerminal-1.0.dmg
downloadtitle: Download v1.0
downloadsubtitle: Requires OS X 10.6 or 10.7
repo: http://github.com/binaryage/totalterminal
meta_title: TotalTerminal is a system-wide terminal accessible via a hot-key
meta_keywords: totalterminal,terminal,osx,simbl,binaryage,productivity,software,visor
meta_description: TotalTerminal is a plugin for Terminal.app which provides Quake-style terminal window available on keyboard shortcut
meta_image: http://www.binaryage.com/shared/img/icons/totalterminal-256.png
facebook: 1
retweet: 1
buzz: 1
fbsdk: 1
flattr: http://totalterminal.binaryage.com
ogmeta: {
    site_name: "BinaryAge website",
    description: "TotalTerminal is a system-wide terminal for OS X available on a hot-key",
    email: "support@binaryage.com",
    type: "product",
    title: "TotalTerminal",
    url: "http://totalterminal.binaryage.com",
    image: "http://www.binaryage.com/shared/img/icons/totalterminal-256.png"
}
shots: [{
    title: "TotalTerminal's Visor window with nice colors!",
    thumb: "/shared/img/totalterminal-mainshot.png",
    full: "/shared/img/totalterminal-mainshot-full.png"
}]
---

## Installation

Installation is easy.

  1. Run installer
  2. Configure your keyboard trigger by selecting the TotalTerminal Status Menu Item -> `Preferences ...` and edit your keyboard hot-key. By default it is `CTRL+~`

Then you can trigger Visor with your hot-key from any application to get an instant terminal session.

To hide Visor, you can either:

  * re-trigger with your key-combo
  * optionally, you can click off the TotalTerminal window

## FAQ

#### I like the idea, but I want to use Terminal.app features. Do you plan to support tabs/unicode/whatever?
> TotalTerminal is just a light-weight plugin for Terminal.app (SIMBL). You should be able to use most of Terminal.app features with TotalTerminal. The only broken feature is "Windows Groups".

#### Does TotalTerminal work on OSX 10.7 (Lion)?
> Yes, since 1.0.

#### Does TotalTerminal work on OSX 10.6 (Snow Leopard)?
> Yes, since 1.0.

#### Does TotalTerminal work on OSX 10.5 (Leopard)?
> It should work, but I no longer support Leopard.

#### Does TotalTerminal work on OSX 10.4 (Tiger)?
> Tiger was supported by early TotalTerminals (pre 1.5).

#### How do I uninstall TotalTerminal?
> You may use Status Menu Item and select `Uninstall TotalTerminal`. Alternatively you may download TotalTerminal DMG again and use Uninstaller which is present there.

#### Where are TotalTerminal settings stored?
> TotalTerminal settings are stored with Terminal.app settings. You can `open ~/Library/Preferences/com.apple.Terminal.plist` and tweak the values (better to do this when Terminal.app is not running). 
If you have troubles with TotalTerminal settings or the generated TotalTerminal profile, delete this file and restart Terminal.app. The file will be recreated with default values.

#### My TotalTerminal menu-bar icon is dimmed out. My hot-key doesn't work and just beeps. What's wrong?
> There can be only one Visor-ed terminal window in the system. If you close this terminal window (for example `Control+D` or typing exit in shell), TotalTerminal gets into the disabled state you are describing. Just open a new terminal window and it gets Visor-ed again. You can do this for example by clicking on Terminal.app icon in the Dock.

#### How can I open a new terminal window the old way, as a classic OSX window?
> If there is a Visor-ed terminal window (TotalTerminal menu-bar icon is active) every new terminal window will be opened as a classic OSX window. In other words, open at least two terminal windows. The second one will be classic terminal window for sure.

#### How can I change the height of Visor?
> Go to Terminal.app's Preferences -> Window -> Rows

#### How can I stick Visor to left screen edge?
> Look for the "Position" option in TotalTerminal Preferences and pick "Left-Stretch" window placement.

#### How can I change the width of Visor?
> By default TotalTerminal window stretches to the full screen width. Set some non-stretching positioning for TotalTerminal window in TotalTerminal Preferences, then Go to Terminal.app's Preferences -> Window -> Columns.

#### Is it possible to show Visor only on a secondary monitor?
> Go to TotalTerminal Preferences -> Screen

#### Is it possible to see Visor on every Space?
> TotalTerminal forces its window to be visible on every space. You may disable this in TotalTerminal Preferences. Note: Spaces configuration for Terminal.app doesn't apply to the visor-ed terminal window, it is effective only for other (classic) terminal windows.

#### I want to keep different preferences for Visor and other (classic) terminal windows. What is the best way manage this?
> There should be a profile in Terminal.app called "Visor". Visor-ed window attempts to use the "Visor" profile for opening new tabs (regardless of "default profile" or "startup profile" settings in Terminal.app).

#### Do I need to install TerminalColours SIMBL with TotalTerminal?
> No, TerminalColours is integrated into TotalTerminal 1.0 and later.

#### Do I need to install CopyOnSelect SIMBL with TotalTerminal?
> No, CopyOnSelect is integrated into TotalTerminal 1.0 and later. It is a configurable option in TotalTerminal Preferences (disabled by default).

## Changelog

<div class="changelogx"></div>

<script type="text/javascript" charset="utf-8">
    $(function() {
        $('.changelogx').load('changelog.html?x='+((Math.random()+"").substring(2))+' #page');
    });
    
    function showBetaHint() {
        $('.betahint').toggle();
    }
</script>

## Links

#### Source code
  * [sources are hosted on GitHub](http://github.com/binaryage/totalterminal)

#### The original TotalTerminal called "Visor" was brought to you by Alcor ([Blacktree](http://blacktree.com)), kudos man!

**[Where can I get older versions?](http://visor.binaryage.com)**

## Special Guest

### Ben Stiglitz about Terminal.app

<div>Ben is the author of Terminal.app. Kudos!</div>
<embed src='http://rubyconf2008.confreaks.com/player.swf' height='260' width='640' allowscriptaccess='always' allowfullscreen='true' flashvars='file=http%3A%2F%2Frubyconf2008.confreaks.com%2Fvideos%2Fterminalapp-small.mp4&image=images%2Fterminalapp-preview.jpg&plugins=viral-1'/>

[darwin]: http://github.com/darwin
[torsten]: http://github.com/torsten
[pumpkin]: http://github.com/pumpkin
[blinks]: http://github.com/blinks
[cglee]: http://github.com/cglee
[kleinman]: http://github.com/kleinman
[genki]: http://github.com/genki
[ciaran]: http://github.com/ciaran
[evanphx]: http://github.com/evanphx
[contribute]: http://github.com/binaryage/totalterminal
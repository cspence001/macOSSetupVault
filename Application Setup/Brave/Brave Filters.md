### Brave Shields Filters and Adblock Lists

##### Brave Default Lists and Directory Catalog 
These lists are built-into Brave natively. 

1. **Default Lists**
   - [Brave Default Lists](https://github.com/brave/brave-browser/wiki/Blocking-goals-and-policy)
2. **Default and Opt-in Catalog**
   - [Brave Default and Opt-in Catalog](https://github.com/brave/adblock-resources/blob/master/filter_lists/list_catalog.json)
3. **Brave Adblock Lists**
   - [Brave Adblock Lists](https://github.com/brave/adblock-lists/tree/master/brave-lists)

##### Opt-in Filter Lists 
`brave://settings/shields/filters` > `Filter lists`
These are the lists I have enabled:

1. **EasyList Cookie** uses:
   - [Fanboy's Cookie Monster](https://secure.fanboy.co.nz/fanboy-cookiemonster_ubo.txt)
   - [uBlock Origin Annoyances Cookies](https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/annoyances-cookies.txt)
   - [Brave Cookie Specific](https://raw.githubusercontent.com/brave/adblock-lists/master/brave-lists/brave-cookie-specific.txt)
2. **Fanboy's Annoyances + uBO Annoyances** uses:
   - [Fanboy's Annoyances](https://secure.fanboy.co.nz/fanboy-annoyance_ubo.txt)
   - [uBlock Origin Annoyances Cookies](https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/annoyances-cookies.txt)
   - [uBlock Origin Annoyances Others](https://raw.githubusercontent.com/uBlockOrigin/uAssets/master/filters/annoyances-others.txt)
3. **Brave Experimental Adblock Rules**
   - [Experimental Adblock Rules](https://raw.githubusercontent.com/brave/adblock-lists/master/brave-lists/experimental.txt)
4. **Brave AdGuard URL Tracking Protection Filters**
   - [AdGuard URL Tracking Protection](https://raw.githubusercontent.com/AdguardTeam/FiltersRegistry/master/filters/filter_17_TrackParam/filter.txt)
5. **Brave Bypass Paywalls Clean** 
   - [Bypass Paywalls Clean](https://gitflic.ru/project/magnolia1234/bypass-paywalls-clean-filters/blob/raw?file=bpc-paywall-filter.txt)

**Notes on opt-in lists**: 
* Brave provides a list for blocking Twitch Ads (`Brave Twitch AdBlock Rules`) that currently sits empty, it contained a scriptlet: `twitch.tv##+js(vaft-ublock-origin)` that has been moved back into `Brave Experimental Adblock Rules`. To block Twitch Ads you can either enable the `Brave Experimental Adblock Rules` or add the scriptlet to your custom filters. 
* `Bypass Paywalls Clean List` supports less sites than the extension; See [[#Bypass Paywalls Chrome Clean Extension]]. If using the extension, it's better to leave this list disabled.
	* These URLs can also be added to filter paywalls in `Shields > Content Filters > Add custom filter lists`:
	1. [Paywall Remover List](https://raw.githubusercontent.com/acnapyx/paywall-remover/master/paywall-remover-list.txt)
	2. [Anti-Paywall](https://raw.githubusercontent.com/liamengland1/miscfilters/master/antipaywall.txt)
* [BraveFilterSearch](https://github.com/cspence001/BraveFilterSearch) is a simple python application I created for reviewing contents of the latest opt-in Brave Filter Lists. You can use this to identify filters for specific websites, identifying scripts that block page elements, debugging ad blockers, comparing Active + Custom Filters, etc. 

---
##### Custom Filters
Custom filters can be created in Brave by navigating to `brave://settings/shields/filters` and selecting `Create custom filters`.
**Note:** Brave is broadly compatible with uBlock Origin's filter rule syntax. 
* [uBlock Origin Static filter syntax page](https://github.com/gorhill/uBlock/wiki/Static-filter-syntax). Differences in compatibility are treated as bugs and are documented in the [adblock-rust issue tracker](https://github.com/brave/adblock-rust/issues).

```ini
! 2023-02-10 github.com
@@||collector.github.com/github/collect$other,first-party
! 2024-05-31 https://www.google.com Block A.I Search results
www.google.com##.M8OgIe > div:nth-of-type(2) > div
! 2023-02-11 duckduckgo.com
@@||improving.duckduckgo.com/t/*$other,first-party
@@||improving.duckduckgo.com/t/trmx$other,first-party
! --- Twitch Ad Block (alt in Experimental Filter List, Brave)
! twitch.tv##+js(twitch-videoad)
! ——— delayed loading
||cdn.ampproject.org/v0.js$script,redirect=ampproject_v0.js
!
! ——— gmail tracking
*/cleardot.gif$image,redirect=1x1.gif
*/cleardot.gif$xmlhttprequest,redirect=1x1.gif
! ——— external images
||googleusercontent.com/proxy$domain=mail.google.com
!
! ——— disable PWA button in Fx mobile
!#if env_mobile
*$csp=manifest-src 'none'
!#endif
!
! ——— youtube
/annotations_module.js$script,important,domain=youtube.com
/endscreen.js$script,important,domain=youtube.com
! seeking
youtube.com##.ytp-time-seeking
! player on channel pages
youtube.com##ytd-channel-video-player-renderer:remove()
! rickroll
dQw4w9WgXcQ$document,xhr,domain=youtube.com
! login
youtube.com##+js(set, ytInitialPlayerResponse.auxiliaryUi.messageRenderers.upsellDialogRenderer.isVisible, false)
youtube.com##+js(set, ytInitialData.overlay.upsellDialogRenderer.isVisible, false)
youtube.com##+js(json-prune, [].playerResponse.auxiliaryUi.messageRenderers.upsellDialogRenderer auxiliaryUi.messageRenderers.upsellDialogRenderer)
! consent
youtube.com##+js(set, ytInitialData.topbar.desktopTopbarRenderer.interstitial.consentBumpRenderer.forceConsent, false)
youtube.com##+js(json-prune, [].response.topbar.desktopTopbarRenderer.interstitial.consentBumpRenderer topbar.desktopTopbarRenderer.interstitial.consentBumpRenderer)
! Consent V2
youtube.com##+js(set, ytInitialData.topbar.desktopTopbarRenderer.interstitial.consentBumpV2Renderer, undefined)
youtube.com##+js(json-prune, [].response.overlay.consentBumpV2Renderer topbar.desktopTopbarRenderer.interstitial.consentBumpV2Renderer overlay.consentBumpV2Renderer response.overlay.consentBumpV2Renderer)
! some videos redirect persistently to consent page:
youtube.com##+js(set, ytInitialData.onResponseReceivedEndpoints, undefined)
! Consent on #shorts
youtube.com##+js(set, ytInitialData.desktopTopbar.desktopTopbarRenderer.interstitial.consentBumpV2Renderer, undefined)
! ——— get rid of subtitles that are force-enabled by default
www.youtube.com##.ytp-caption-segment,.ytp-subtitles-button
!
! ——— old reddit
reddit.com##.redesign-beta-optin
reddit.com##.gilded-icon, .give-gold-button
! ——— banner "You seem to have the hang of this"
reddit.com##.onboardingbar
! ——— specific cookie notice for mobile
!#if env_mobile
reddit.com##.EUCookieNotice
!#endif
!
// Reddit Telemetry
! ||redditstatic.com/shreddit/en-US/*telemetry*.js
!
! ——— only selected langs in sidebar
wikipedia.org###p-lang ul > .interlanguage-link:not(.interwiki-en):not(.interwiki-pl)
!
! ——— mark visited bugzilla links in "The first official ... builds are out"
forums.mozillazine.org##a:visited[href^="https://bugzilla.mozilla.org/show_bug.cgi"]:style(color: red !important;)
! ——— next thread links in current tab
forums.mozillazine.org##+js(remove-attr, target, a[target="_blank"])
!
! ——— disqus, annoying emoticons
disqus.com###reactions
!
! ——— uAssets/commit/bf477ba87882c8eb64861b7e0b95e023475fd4c0
||report-uri.com^$all,important
!
! ——— internal benchmark, http://localhost:8080/requests_top500.json
@@||localhost:8080/requests_top500$xhr
!
! ——— noise in mod log
reddit.com###siteTable.modactionlisting.sitetable .modactions.editflair:nth-ancestor(2)
!
! ——— exploitation of users for code testing
||sentry.io^$3p,important
||bugsnag.com^$3p,important
!
! ——— IDN protection https://twitter.com/gorhill/status/1422914791077732352
||xn--$doc,frame
!
! ——— annoying scrolling
morele.net##+js(set, scrollTo, noopFunc)
!
! ——— Telepolis
! block images on home page
|https://www.telepolis.pl/|$csp=img-src 'none'
! block auto pagination
||www.telepolis.pl/next/*$xhr,domain=www.telepolis.pl
```

---
##### Bypass Paywalls Chrome Clean Extension
https://gitflic.ru/project/magnolia1234/bpc_uploads
* Load this extension unpacked in Brave. An experimental opt-in list can be enabled in Brave `Settings > Shields > Content Filters > Filter lists` but does not block as effectively as the extension.
1. **Supported Websites and Installation Instructions**
   - [Extension README](chrome-extension://lkbebcjgcmobigpeffafkodonchffocl/README.html) Follow instructions for `Load unpacked: Chrome, MS Edge or Brave (all desktop)`
2. **Python Script for Updating the Extension**
   - [Update Script](https://gist.github.com/cspence001/33302cf180212a22423e4abcd2aa7868) python script I created for updating the extension.
     - Configure `DOWNLOAD_DIR` to match the installation location of the extension.
     - Create a shell alias for updates: `alias bpcupdate="python3 ~/macsetup/_scripts/bpcgitflic.py"`
     - Run updates with `$ bpcupdate`.


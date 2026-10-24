# Xiaomi Cockpit website: project notes for Claude

Read this first. Public website of the app "Xiaomi Cockpit" (https://xiaomi-cockpit.github.io), repo `xiaomi-cockpit/xiaomi-cockpit.github.io`, GitHub Pages from `main` (root). The app itself lives in the PRIVATE repo `xiaomi-cockpit/xiaomi-cockpit` (moved from `pagnchanak/xiaomi-cockpit`) (see its CLAUDE.md for what the app does); this repo has no app code.

## Who you work with
- The user has no coding knowledge: explain in plain language, give exact click steps, do the coding yourself. They use Windows and the GitHub website.
- Work on `main` directly (small static site). Push, then check https://xiaomi-cockpit.github.io a minute later.
- You cannot create releases, delete branches or change repository/Pages settings from the session; tell the user the clicks.

## Purpose
Showcase the app: features, download + install instructions, a detailed user guide. English only for now (languages can be added later). Easy to extend with more info.

## Files
- `index.html`: hero with a drawn preview of the dashboard, features, the three layouts, download + install steps, FAQ.
- `guide.html`: detailed user guide (sticky contents list, sections: getting started, layouts, gestures, clock, music, map and navigation incl. Google key, dock, floating button, settings, looks, troubleshooting, updating). Keep it in sync with the app (see the app repo's CLAUDE.md).
- `404.html`, `.nojekyll`, `assets/style.css`, `assets/site.js`, `assets/icons.svg` (sprite, `<use href="assets/icons.svg#i-name">`), `assets/favicon.svg`.
- Add screenshots later under `assets/screenshots/` and show them in a gallery section.

## Design (same look as the app)
- Dark and light themes: follows the system, the header button toggles and remembers (localStorage `cockpit-theme`); `?theme=light` / `?theme=dark` in the URL overrides (handy for screenshots). The inline script in each page head sets the theme before paint.
- Background = the app's Cockpit style drawn on a canvas (`#bg`, `assets/site.js`): dark gradient `#0a1621 -> #102232` with blue light pools, light solid `#a7b4c1` (the app's light Cockpit colour dimmed to 86%), three large soft accent bubbles drifting (dark: accent shades 1 / 0.63 / 0.88, alpha 120/95/105; light: accent mixed 0 / 0.12 / 0.06 with white).
- Accent colour cycles every 7 s through the app's accent list (dark: `#7fe3c4 #7cc7ff #b9a3ff #ff9ec4 #ffb36b #f4db6a #ff8e86 #b4e86a`; light: `#14a383 #1a7fd6 #6a4bd6 #d6336f #d9650f #a77f00 #d23a2f #4d8f12`), blended smoothly; CSS variables `--accent`, `--accent-rgb`, `--on-accent` (dark ink on bright accents).
- Glass panels (`.glass`, `.glass.strong`): backdrop blur + saturation, translucent tint (dark 7.5% / light 50%), 1 px rim, a lit top edge and top-left curve (`::after`), a soft shadow ONLY under the bottom edge (box-shadow with negative spread), and a live accent glow on the rim where a background bubble crosses the edge (`::before`, driven by `updateGlow()` setting `--ga --gx --gy`). Corner radius 22 px.
- Typography: system font stack, light-weight large headings, small spaced caps labels (`.tiny`). `prefers-reduced-motion` stops the animation and reveals.

## Links
- Download button (always the newest APK): `https://github.com/xiaomi-cockpit/xiaomi-cockpit.github.io/releases/latest/download/Xiaomi-Cockpit.apk`. The app repo's workflow publishes each release's APK to a release in THIS public repo (secret `WEBSITE_TOKEN` in the app repo). Version badge: JS reads `releases/latest` of this repo.
- Not affiliated with Xiaomi, Baidu or Google (footer text); keep that disclaimer.

## Testing
No local build needed. To look at the pages: `python3 -m http.server`, then headless Chromium (`/opt/pw-browsers/chromium-*/chrome-linux/chrome --headless=new --no-sandbox --screenshot=out.png --window-size=1440,2000 --virtual-time-budget=5000 URL?theme=light`). Headless Chromium has a minimum window width of about 500 px; check phones through an iframe of 390 px.

## Update (v1.3.0 content)
- index.html: new feature cards (Weather you can see, Nearby EV chargers, Report and request; icon `i-bolt` added to the sprite), updated Clock/weather and Navigation cards, Google key card/FAQ (chargers, Google Weather), first-start step (language + map service), new FAQ "Can I ask for a new feature?".
- guide.html: sections Weather effects (#weather-fx) and Nearby chargers (#chargers), route overview + live traffic update, one Google key (Settings > Google key, four services incl. Weather API), weather source/frequency, Home/Work/Search buttons, settings table, Report/Request captions.

## Update (v1.4.0 content)
- guide.html: next events (up to 3 rows), music bars without the visualizer setting, floating button off by default and never on the phone screen, glass shadow/refraction switches removed, self-update (Settings > Updates). index.html music card text updated.

## Update (docs for v1.4.0)
- Full pass over guide.html and index.html for v1.4.0: first start (language + map service), permissions table (calendar events, install unknown apps), weather effects on by default, nearby chargers (15 closest, no limit, connector types, place line), settings table (Updates section, Diagnostics wording), glass effects always on, extra FAQs (chargers empty, update does not install, Google Weather note), home page (three events, data sources, self-update). Keep both pages in step with the app's CLAUDE.md after every release.

## Languages (website)
- The site is available in English (default), Khmer (km), Russian (ru) and Polish (pl), like the app. `assets/i18n.js` translates the page in the browser: the KEY of each string is its English HTML (inline tags kept, icons as `{{ic:id}}`), the dictionaries are `assets/i18n/km.json`, `ru.json`, `pl.json` ({English HTML: translation}). Language = `?lang=` > saved choice (localStorage `cockpit-lang`) > browser language. A switcher (`#lang-select`) is added in the header.
- When English text on a page changes or is added, add/update the same English string as a key in all three JSON files. To list the exact keys of a page, open it with `?dumpkeys=1` (a JSON of units/attrs/metas is appended). Untranslated text simply stays English.
- Mark things that must never be translated with `translate="no"` or `aria-hidden="true"`. Translations were written by Claude and are welcome for native-speaker review.
- Asset links in the HTML carry ?v=<timestamp>; change it whenever style.css/site.js/i18n.js change, so phones do not keep old copies.

## Update (v1.6.0 content)
- Yandex API map service documented: guide (#yandex-key: get a key, status colours, black-map FAQ), map service list, route/chargers notes, settings table, first-start texts; home page (Live map and Navigation cards, optional-key card, new FAQ "Can I use Yandex maps instead of Google?", footer data sources).
- Theme rules (Theme setting wins; only Light lets weather effects darken; live switching with a fade), weather wording from Google Weather, key status colours (green/orange/red) described in the guide.
- Hero preview: weather text "Cloudy" (Google wording) and the map box has a faint accent rim (`.mmap::after`, masked to two corners like a bubble crossing the edge).
- Translations (km/ru/pl JSON) added for every new string; the JSON files are one entry per line (keep that format).

## Update (7 October 2026): screenshots, new features, privacy
- The hero and the Screens section are illustrations again (screenshots and renders were tried and rejected as not doing the app justice; the files are deleted). The hero is HTML/CSS/SVG (`.carscreen .screen`, markup in index.html) and `site.js` cycles its state class every 4.2 s: `s-nav` (route, turn banner with the Then line, trip card), `s-alt` (grey alternative with a minutes bubble), `s-chargers` (green pins, station bar, card; `add` = the card says Add and the blue stop appears), `s-notif` (notification card), `s-edit` (dock wobbles with remove badges and +). Keep it in step with the app when the UI changes. The three Screens illustrations are inline SVGs generated for the layouts (classes `.il ...` at the end of style.css).
- Guide, features, install steps, FAQ and privacy were brought in line with the app after 1.6.0 (setup wizard, consent and API status, Navigation SDK banner, stops and alternatives, charger flow, notification card, dock edit mode, Settings sections Permissions/Service/Weather/Map/Music/Look, no Gemini, no Custom Maps App). All new English text has km/ru/pl entries; `assets/i18n/*.json` were pruned to the keys that exist on the pages (check with `?dumpkeys=1`). Asset links use `?v=202610071200`.

## Update (2.0.0)
- Stops are red numbered discs in the hero and the Screens illustrations; the alternative-route icon now matches the app's (`Glyph.ALT`: a stem splitting into two arrows, the left branch dashed). The Telegram banner in the app repo (`.github/banner/release-banner.html`) uses a copy of the hero illustration: keep them in step.

## Update (2.1.0)
- Guide and features: the weather no longer changes the theme or the map (gloomy/night wording removed), Night map (Auto/On/Off), Weather animation override, the cover tile and tap/long-press on the music widget, and the four new long presses in the gestures table. All new texts have km/ru/pl entries; the key check (`?dumpkeys=1`) reports nothing missing except the logo.

## Update (2.2.0)
- Guide: clock card taps (clock = 12/24 h, weather = refresh now, events = calendar sync) in the gestures table and the clock section, charging bolt / green ring on the status gauge, weather from Google or Open-Meteo with any map service and "Use my location" (no city = follows the phone's location, name from OpenStreetMap), Night map Auto (daylight) / Theme / On / Off, live traffic refresh for Google too (Settings > Map > Live traffic update, 30 s / 1 / 2 default / 5 min), new section `#usual-routes` (route learning on the phone only, "Your usual route" / "Usual", Settings > Map > Route learning, Diagnostics "Learned drives"), music waves fan out, search field brings the keyboard back, Diagnostics shows only the chosen map service, FAQ about Play Store offering Baidu as an "update" (fixed by 2.2.0's higher internal version).
- Home page: clock, music, live map, navigation and chargers cards (drive time instead of power), optional-key card, setup step 6, Google key FAQ, new Play Store / Baidu FAQ, footer "Google or Open-Meteo".
- Privacy (last updated 20 October 2026): route-learning drives stay in a local file (deleted with the switch or on uninstall), automatic weather location goes to Open-Meteo / Google and to Nominatim for the place name, calendar sync tap only asks Android to sync. All new text has km/ru/pl entries; old keys removed (dumpkeys check clean except the logo; 404.html was never translated).

## Update (2.3.0)
- Guide: new section `#backup` "Backup and restore" (last in the contents list; Settings > Backup: Back up settings with "Without my API keys" / "With my API keys", saved with Android's file picker anywhere, e.g. Google Drive; Restore settings = pick file, confirm, app restarts; file holds settings, dock, Home/Work, recent places, background photo, learned routes; permissions are not restored). Install steps mention the setup's "Restore a backup?" page (Choose backup file restores and ends the setup, Skip goes on to the service). Settings table has a Backup row. Dock: work-profile and Xiaomi dual apps (badge or "(second copy)"). Google key tip: package name + each SHA-1 shown in the setup's Google key guide (two, with Copy buttons); a second tip tells people who restricted their key before 2.3.0 to add the second SHA-1 (the old one keeps working). Updating: 2.3.0 has a new signing key, updates still install over the old app. Never print the SHA-1 values on the site.
- Home page: dock card (work profile and dual apps), Guided setup card and install step 3 (restore a backup during the setup), new FAQ "How do I move my settings to a new phone or a new install?".
- Privacy (last updated 21 October 2026): new section `#backup` "Backup files" (created only on request, saved where the user chooses, never sent, keys only with "With my API keys") and a Your choices bullet.
- Permissions lists never had the microphone; nothing to remove. km/ru/pl: 461 keys each, dumpkeys clean except the logo. Asset `?v=` unchanged (no CSS/JS change).
- Screenshot tip: headless Chromium does not paint below ~16384 px and ignores `#hash` with `--screenshot`; to check a section near the end of guide.html, make a temporary copy with the earlier sections cut out (do not commit it).

## Update (3.0.0)
- Guide and home page for 3.0: package name com.baidu.BaiduMap (full screen while driving; Baidu Maps and Mi Dashboard must not be installed), new feature card "Full screen while driving", FAQ "I have version 2. How do I move to 3.0?", guide `#move3` "Moving to version 3.0" (in Updating), troubleshooting "Routes and search stopped working after moving to 3.0", guide `#banner` is now "Google's navigation screen" (Google's arrow, turn banner, separate Then banner, arrival card, trip buttons on the right, buttons fade while driving), music ribbon in album colours, setup "your look" page, backups always encrypted with the keys (privacy updated, 22 October 2026).
- Hero illustration: Google's turn banner at the top left with its own "Then" pill under it, the arrival card (green minutes) at the bottom left, the trip buttons (✕, alternatives, +) stacked on the right above expand, map buttons faded while navigating, blue chevron car, plain (text-coloured) bolt, music visualizer = twisted six-line ribbon in two colours with rising dots. Layout illustrations got the chevron car and ribbon too. km/ru/pl: 482 keys each, dumpkeys clean except the logo. Asset `?v=202610221500`.

## Update (real screenshots in Screens, 24 October 2026)
- The Screens section shows the user's real, FULL car screenshots (car status bar, car panels and bottom bar included) instead of the drawn layouts: `assets/screenshots/size1|size2|size3-light|dark.webp` (1800x1125). Each figure has a light and a dark image (`.shot .sl` / `.sd`, each a link to the full image); CSS shows the one matching the site theme; the three columns are equal (`.sizes.shots`). Cleaned before use: the place labels and pins on the map were removed (each replaced with a matching piece of nearby map, the map itself is NOT blurred or mirrored: the user disliked both), calendar events and the district name are blurred, and the car's profile picture is replaced with a generic avatar. Never publish map labels, calendar or location details, or the avatar. The alt texts reuse the old aria-label strings (already translated). The hero is still the drawn illustration. `?v=202610241500` for style.css.

## Update (new icon, hero refresh, 25 October 2026)
- All icons (favicon.svg/.ico, apple-touch-icon, icon-192/512, brand downloads) are the 3.1.1 app icon (gauge ticks + MI squircle).
- Hero: Khmer song "ចម្ប៉ាបាត់ដំបង" / artist "ស៊ូលី ផេង" (`.km`, `translate="no"`), filled cog for the map settings button (same shape as the app's `Glyph.GEAR`, also `i-gear` in the sprite), dock = apps centred with only the + at the end, shining dots in the dark theme (`.dot.dsh`, `#dshine`), notification card at the top centre of the WHOLE screen (`.screen > .notif`) over a dim scrim (`.nscrim`). The Telegram banner in the app repo was updated the same way.

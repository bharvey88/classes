# Intro to Home Assistant

*Smart Homes Done the Right Way · Speaker Cue Outline*

*Format: Talk with hardware props, no live system on screen · Audience: complete beginners · Venue: Dallas Makerspace · Duration: ~95 min of content in a 2-hour slot, 5-min stretch break*

---

## Block 1: The Hook: Home Assistant Alongside Apple, Google, and Alexa (5 min)

- Cold open, no intro, no slides
- **Hands up**: Echo? Google speaker? HomeKit? More than one?
- *"Unfortunately, your home still isn't very smart."*
- **The pain**: an app per room, works with one assistant but not the other, features pulled in an update, dead when the internet dies
- **Nobody throws anything out tonight**: all three are easy to start, each only talks to its own stuff
- **The turn**: HA goes underneath; devices connect to HA, HA hands them back to Alexa, Google, or Apple, keep the speaker you already use
- **What that buys**: one place everything meets, automations across brands, keeps working offline
- **Proof**: mailbox opens → camera snapshot → phone notification → AOL "You've got mail" on every speaker → *"Nobody sold me that. I built it in an afternoon. Let me show you how we got here."*
- Gear on the table all night, picked up as it comes up

## Giveaway Announcement (~1 min)

- *"One of you is taking something home tonight. I'm not telling you what it is until the drawing."*
- **Must be present to win.** The drawing happens at the end of the night.
- Winner drawn from a name wheel; get first names as people arrive
- Milk the mystery: mention the box exists, don't open it, move on.
- **The class URL** (classes.smarthomesellout.com): whiteboard before doors open, say it twice. *"Everything tonight is on this site. The handout has every link and gear pick, so follow along on your phone if you want. Nobody needs to take notes."*

## Block 2: What is Home Assistant? (8 min)

- Brief history: started 2013, Paulus Schoutsen
- **The core idea: HA is the middleman.** One app in the middle, every protocol (Zigbee, Z-Wave, Wi-Fi, Bluetooth) connected to it, instead of ten vendor apps that don't talk to each other.
- **Open Home Foundation**: vendor-neutral, privacy-first, open source
- **Nabu Casa**: commercial arm, funds development, HA Cloud (remote access, Alexa/Google, voice); hold up the hardware
- **Apollo Automation**: *"I work here. We make ESPHome-based sensors, and we're one of the companies Nabu Casa is trusting to help monetize and grow ESPHome commercially."*
- The ecosystem, three tiers (openhomefoundation.org/projects):
    - **Owned outright:** Home Assistant, ESPHome, Music Assistant
    - **Collabs** (partners, not owned): Zigbee2MQTT, WLED (the panel in the corner), OpenDisplay
    - **Standards, drivers, libraries** (250+): Z-Wave JS, HACS, zigpy, Improv Wi-Fi, ESP Web Tools, Piper
- Philosophy: local-first, no vendor lock-in, your data stays yours
- **aio-libs closer**: aiohttp, yarl, multidict and friends, how HA talks to every device at once without choking; volunteers keep them alive → donate link in handout

## Block 3: How to Run It (6 min)

- **Why HAOS**: easiest, best supported, full app ecosystem
- Hardware:
    - **HA Green**: official plug-and-play
    - **Raspberry Pi**: popular, well documented
    - **A used mini PC or an old laptop**: needs 6th-gen Intel or newer; a laptop works even with a dead/no screen (headless); mini PC e.g. Lenovo M910q or similar
    - **Heads up:** mini PCs used to be a lot cheaper before the AI boom drove demand up
    - **N100/N150 mini PCs**: new off Amazon, great value + headroom
- **Advanced options**: running a VM on a server, or a Docker container

## Block 4: Core Concepts + Good Integrations (13 min)

- **Integrations**: how HA connects to devices & services
- **Add-ons / Apps**: extend HA (Mosquitto, ESPHome), "app" terminology
- **Cloud vs Local**: works without internet, privacy, speed, no subscription
- **Integrations worth knowing** (the "it does *that*?" list):
    - **UniFi / UniFi Protect**: network presence + cameras
    - **WLED**: addressable LEDs; runs on certain smart bulbs or LED controllers; the panel in the corner is running it, HA can drive it
    - **Z-Wave**: bring your Z-Wave gear into HA
    - **ESPHome**: your own DIY sensors/devices into HA
    - **Alarmo**: full alarm system, free
    - **BTHome**: local Bluetooth sensors, no cloud
    - **Frigate**: local AI camera / object detection
    - **HomeKit Device**: pair Wi-Fi devices into HA via the HomeKit protocol
    - **HomeKit Bridge**: push HA devices *out* to Apple Home for Siri control; local and free (Alexa/Google need HA Cloud); callback to the opening
    - **Plex**: *"I use it to play media on my TVs as part of morning/night routines"*
    - **Team Tracker**: the fun one: flash LEDs or fire an effect when your team scores a touchdown; build DIY scoreboards with ESPHome or WLED firmware on a HUB75 LED matrix

## Block 5: Protocols (15 min)

- **Z-Wave**: mesh, licensed spectrum, very reliable, needs a controller
    - **Best controller by far: Home Assistant Connect ZWA-2** (not a generic Zooz stick); hold it up
- **Zigbee**: mesh, open standard, huge ecosystem, needs a coordinator
    - **ZHA** (built-in) vs **Zigbee2MQTT** (independent, broadest device support)
    - Your story: Hue bulbs paired straight to Z2M, no bridge, no account, no cloud
    - Gear: Hue bulbs, Aqara (T1M, door/window sensors, Magic Cube), Third Reality
    - **New IKEA stuff**: newer Thread-era devices can still pair over Zigbee via Z2M (verify models)
    - Recommended coordinator: **Home Assistant Connect ZBT-2**, next to it on the table
- **Wi-Fi / Ethernet**: direct IP, no hub
- **Bluetooth / BLE**: short range, passive sensors, proxies
- **Thread & Matter**: honest but gentle. Promising, but still working through firmware/reliability growing pains that Zigbee and Z-Wave sorted out years ago. Beginners can wait.

## Bulbs vs Switches vs Relays (~5 min)

- Frame it as its own moment: this is the buying decision beginners get wrong most often
- **Smart bulbs**: per-bulb color + dimming; downside: flip the wall switch and the bulb loses power and vanishes from HA
- **Smart switches**: replace the wall switch, keep normal bulbs, physical switch still works (spouse-friendly); no per-bulb color
- **In-wall relays (Zooz, Shelly)**: hide behind your existing switch; keep the fixture and wall control, just add brains. Best for retrofits and dumb fixtures
- Each comes in Zigbee / Z-Wave / Wi-Fi (Zooz for Z-Wave, Shelly for Wi-Fi; Shelly is the popular pick)
- **Rule of thumb:** switches & relays keep the wall working for family/guests; bulbs are for color everywhere

## Q&A Stop 1 (3 min)

Short questions on the spot, longer ones parked to the end.

## Block 6: HA Walkthrough (10 min)

- Skip onboarding: describe a working system, that's what sells it
- **Dashboard deep-dive**: sections, cards, the view they'd actually use at home (skip the menu tour)
- **HACS (Home Assistant Community Store)**: custom cards, themes, community integrations beyond what's built in
    - Theme worth naming: **Catppuccin Macchiato**
    - Caveat: HACS is foundation-governed, what you install through it is not; install sparingly
- **Companion app (iOS/Android)**: install, sign in, presence/device tracking, push notifications
- **Adding integrations**: a couple of clicks in Settings, most want an account or API key; point at the list on the site
- **Create a simple automation**: turn a light on/off with a Zigbee button, with the button in your hand
- The App store: Mosquitto, ESPHome, File editor

## Break (5 min)

Enter the names into the wheel.

## Block 7: Automations & Scripts: The Payoff (8 min)

- **Automations = "when X happens, do Y"**: they run themselves
- **Your real examples:**
    - **Mailbox alert**, the one from the opening: mailbox opens → camera snapshot → phone notification with the image + "You've got mail" → also pops on the desktop PC and the TVs (if online) → plays the old AOL "You've Got Mail" clip on the HomePods around the house; one trigger, four actions, pieces on the table
    - **Leaving home** (device tracker): I leave → lock doors + arm alarm → snapshot of the room → lights brightness/color off then back on → sleep the PC
- **Scripts = a saved routine you trigger on demand** (vs automations that fire themselves)
    - Your wake-up routine and goodnight/sleep routine scripts
    - Can be fired by a button, voice, the dashboard, or called by an automation
- **Takeaway:** automations react, scripts are reusable routines, and they combine

## Block 8: ESPHome & Building Your Own (4 min)

- What it is: turns cheap ESP32 chips into HA devices using YAML
- Part of the ecosystem, under the Open Home Foundation like HA itself
- **Apollo Automation** builds on it (your gear)
- **ESPHome Starter Kit**: upcoming on-ramp into ESPHome → *"I've got a beta version here to show you"* (pass it around)
- Full hands-on build is **its own future class**, tease it

**ESPHome reference notes (keep for the future class, not spoken here):**

- How it integrates with HA, what YAML is, what a "component" is
- **ESP32 variants**: WROOM32 (original, solid general purpose) / C3 (RISC-V, single core, budget, simple sensors) / C6 (native Thread + Zigbee, Wi-Fi 6) / S3 (dual core, more GPIO, PSRAM, heavier workloads)
- **PSRAM**: what it is, when you need it (WLED, driving TFT/e-ink displays)
- **Flash**: 4MB vs 8MB and OTA implications
- **Why not ESP8266**: slow, low flash, missing critical peripherals (I2C, LEDC, multiple UARTs); ESP32 is cheap enough there's no reason to deploy 8266 today
- Why the ESPHome add-on in HAOS makes onboarding/flashing easy
- **Future-class build:** ESP32 WROOM32 + DHT22 + 5-pixel WS2812B strip: YAML walkthrough, flash, watch it appear in HA, dashboard + light effects

## Block 9: More Ways to Interact: Voice & Wall Displays (4 min)

- **HA Voice Preview Edition**: local voice assistant hardware; pass it around here
    - Honest caveat: replacing/mimicking Alexa or Google takes real manual setup today, not plug-and-play yet
- **Wall tablets + Fully Kiosk**: cheap tablet as a wall-mounted dashboard
    - Fully Kiosk handles screen on/off (motion wake) + locked-down kiosk mode

## Block 10: Claude Code + HA MCP (6 min): the finale

- What it is: an AI agent that talks directly to your HA through the MCP
- Configure devices, build dashboards, write automations by *asking*
- **Before and after**: what you typed, what came back, how long it took; result open on the laptop, send it down the front row
- Why it's a big deal for beginners: lowers the wall between "I have an idea" and "it's running"

## Block 11: Where to Go Next (3 min)

- **HA Docs**: the integration library is your best friend
- **ESPHome.io**: component reference (and tease the ESPHome class)
- **Community**: ESPHome Discord, r/homeassistant, r/esphome
- **Quality YouTube**: digiblur and other rigorous folks
- **Influencer warning**: sponsorships/affiliate links, cross-reference the community
- **Content decay warning**: HA moves fast, check dates, go straight to official docs

## The Giveaway Drawing (~2 min)

- Reveal the prize now, then draw the winner.
- **Name wheel** (wheelofnames.com) on the laptop: names entered during the break, hold it up, spin, read the winner out loud
- Don't write the actual item in this outline. The outline is public and the mystery is the fun part.
- If the prize happens to be something with a voice, letting it announce its own winner is a great reveal (cut the gag if running long).

## Q&A Stop 2 (8 min)

*Running time: ~94 min content + 5 break ≈ 100 min, scheduled in a 2-hour slot. Q&A, the drawing, and overruns live in the buffer.*

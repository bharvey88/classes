# Intro to Home Assistant

*Smart Homes Done the Right Way · Speaker Cue Outline*

*Format: Presentation + live demo · Audience: complete beginners · Venue: Dallas Makerspace · Duration: about 2 to 2.5 hrs with a 10-min break*

---

## Block 1: The Hook (5 min)

- Cold open, no intro, no slides
- Dashboard already live on screen
- Fire something visual + physical in front of them
- Let "wait, what is that?" hang → *"Let me show you how we got here."*
- Plant the giveaway picker: show a premade "winner picker" card: *"Rolls a random number. We'll use it a few times tonight, hang on to your ticket."*

**Demo options to pick from later:**

- **WLED strip/bulb**: on/off, color, an effect. Loudest visually, instant. Best crowd-pleaser, foreshadows ESPHome.
- **Aqara Magic Cube**: flip/rotate/shake → action. Tactile "it's not even an app" magic.
- **Smart button** (Aqara / Third Reality): single/double/long press → action.
- **Hue**: reliable, less novel; good backup.
- **Best combo:** Magic Cube gesture → WLED color change. Physical in → visible out.
- **Logistics:** pre-stage on dashboard, confirm on network, test before doors open, manual toggle fallback (makerspaces are RF-noisy).

## Giveaway Announcement (~1 min)

- *"Nabu Casa sent us four things to give away tonight: an HA Green, a ZWA-2 Z-Wave controller, a ZBT-2 Zigbee coordinator, and a Voice Preview Edition."*
- **Must be present to win.** Drawings happen throughout as each one comes up.
- Winner picked live by the random-number roller you just saw.

## Block 2: What is Home Assistant? (12 min)

- Brief history: started 2013, Paulus Schoutsen
- **The core idea: HA is the middleman.** One app in the middle, every protocol (Zigbee, Z-Wave, Wi-Fi, Bluetooth) connected to it, instead of ten vendor apps that don't talk to each other.
- **Open Home Foundation**: vendor-neutral, privacy-first, open source
- **Nabu Casa**: commercial arm, funds development, HA Cloud (remote access, Alexa/Google, voice)
- **Apollo Automation**: *"I work here. We make ESPHome-based sensors, and we're one of the companies Nabu Casa is trusting to help monetize and grow ESPHome commercially."*
- The ecosystem, two flavors:
    - **Nabu Casa stewards:** ESPHome, Z-Wave JS, Music Assistant
    - **Independent projects they fund/donate to:** Zigbee2MQTT
- Philosophy: local-first, no vendor lock-in, your data stays yours
- **aiohttp closer**: "how HA talks to all your devices at once without choking; volunteers keep it alive" → donate link in handout

## Block 3: How to Run It (8 min)

- **Why HAOS**: easiest, best supported, full app ecosystem
- Hardware:
    - **HA Green**: official plug-and-play
    - **Raspberry Pi**: popular, well documented
    - **A used mini PC or an old laptop**: needs 6th-gen Intel or newer; a laptop works even with a dead/no screen (headless); mini PC e.g. Lenovo M910q or similar
    - **Heads up:** mini PCs used to be a lot cheaper before the AI boom drove demand up
    - **N100/N150 mini PCs**: new off Amazon, great value + headroom
- Why we're skipping Docker / venv / manual today
- **GIVEAWAY: HA Green:** *"the exact thing I'm recommending, let's roll for it right now."* (use the picker)

## Block 4: Core Concepts + Good Integrations (13 min)

- **Integrations**: how HA connects to devices & services
- **Add-ons / Apps**: extend HA (Mosquitto, ESPHome), "app" terminology
- **Cloud vs Local**: works without internet, privacy, speed, no subscription
- **Integrations worth knowing** (the "it does *that*?" list):
    - **UniFi / UniFi Protect**: network presence + cameras
    - **WLED**: addressable LEDs; can run on certain smart bulbs or on LED controllers
    - **Z-Wave**: bring your Z-Wave gear into HA
    - **ESPHome**: your own DIY sensors/devices into HA
    - **Alarmo**: full alarm system, free
    - **BTHome**: local Bluetooth sensors, no cloud
    - **Frigate**: local AI camera / object detection
    - **HomeKit Device**: pair Wi-Fi devices into HA via the HomeKit protocol
    - **HomeKit Bridge**: push HA devices *out* to Apple Home for Siri control
    - **Plex**: *"I use it to play media on my TVs as part of morning/night routines"*
    - **Team Tracker**: the fun one: flash LEDs or fire an effect when your team scores a touchdown; build DIY scoreboards with ESPHome or WLED firmware on a HUB75 LED matrix

## Block 5: Protocols (15 min)

- **Z-Wave**: mesh, licensed spectrum, very reliable, needs a controller
    - **Best controller by far: Home Assistant Connect ZWA-2** (not a generic Zooz stick)
    - **GIVEAWAY: ZWA-2** (use the picker)
- **Zigbee**: mesh, open standard, huge ecosystem, needs a coordinator
    - **ZHA** (built-in) vs **Zigbee2MQTT** (independent, broadest device support)
    - Your story: Hue bulbs paired straight to Z2M, no bridge, no account, no cloud
    - Gear: Hue bulbs, Aqara (T1M, door/window sensors, Magic Cube), Third Reality
    - **New IKEA stuff**: newer Thread-era devices can still pair over Zigbee via Z2M (verify models)
    - **GIVEAWAY: ZBT-2** (Zigbee coordinator) (use the picker)
- **Wi-Fi / Ethernet**: direct IP, no hub
- **Bluetooth / BLE**: short range, passive sensors, proxies
- **Thread & Matter**: honest but gentle: promising, but still working through the firmware/reliability growing pains that Zigbee and Z-Wave already sorted out years ago. Beginners can wait.

**Bulbs vs Switches vs Relays (~5 min)**

- **Smart bulbs**: per-bulb color + dimming; downside: flip the wall switch and the bulb loses power and vanishes from HA
- **Smart switches**: replace the wall switch, keep normal bulbs, physical switch still works (spouse-friendly); no per-bulb color
- **In-wall relays (Shelly)**: hide behind your existing switch; keep the fixture and wall control, just add brains. Best for retrofits and dumb fixtures
- Each comes in Zigbee / Z-Wave / Wi-Fi (Shelly is the Wi-Fi go-to)
- **Rule of thumb:** switches & relays keep the wall working for family/guests; bulbs are for color everywhere

## Q&A Stop 1 (5 min)

Short questions on the spot, longer ones parked to the end.

## Block 6: Live HA Demo (22 min)

- Onboarding walkthrough
- **Dashboard deep-dive**: sections, cards, building a usable view (not just a tour)
- **HACS (Home Assistant Community Store)**: custom cards, themes, community integrations beyond what's built in
    - Live: download a theme, **Catppuccin Macchiato**
    - Light caveat: third-party, install sparingly
- **Companion app (iOS/Android)**: install, sign in, presence/device tracking, push notifications
- **Add an integration live**: keep it simple: AirNow (air quality)
- **Create a simple automation**: turn a light on/off with a Zigbee button (may change later)
- Show the App store

## Break (10 min)

## Block 7: Automations & Scripts: The Payoff (10 min)

- **Automations = "when X happens, do Y"**: they run themselves
- **Your real examples:**
    - **Mailbox alert**: mailbox opens → camera snapshot → phone notification with the image + "You've got mail" → also pops on the desktop PC and the TVs (if online) → plays the old AOL "You've Got Mail" clip on the HomePods around the house
    - **Leaving home** (device tracker): I leave → lock doors + arm alarm → snapshot of the room → lights brightness/color off then back on → sleep the PC
- **Scripts = a saved routine you trigger on demand** (vs automations that fire themselves)
    - Your wake-up routine and goodnight/sleep routine scripts
    - Can be fired by a button, voice, the dashboard, or called by an automation
- **Takeaway:** automations react, scripts are reusable routines, and they combine

## Block 8: ESPHome & Building Your Own (6 min)

- What it is: turns cheap ESP32 chips into HA devices using YAML
- Part of the ecosystem, Nabu Casa stewards it
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

## Block 9: More Ways to Interact: Voice & Wall Displays (8 min)

- **HA Voice Preview Edition**: local voice assistant hardware
    - Honest caveat: replacing/mimicking Alexa or Google takes real manual setup today, not plug-and-play yet
    - **GIVEAWAY: Voice PE** (use the picker; if you like, have the Voice PE announce its own winner out loud)
- **Wall tablets + Fully Kiosk**: cheap tablet as a wall-mounted dashboard
    - Fully Kiosk handles screen on/off (motion wake) + locked-down kiosk mode

## Block 10: Claude Code + HA MCP (10 min): the finale

- What it is: an AI agent that talks directly to your HA through the MCP
- Configure devices, build dashboards, write automations by *asking*
- **Live moment if you're brave:** "make me a dashboard card for the living room" → watch it happen
- Why it's a big deal for beginners: lowers the wall between "I have an idea" and "it's running"

## Block 11: Where to Go Next (5 min)

- **HA Docs**: the integration library is your best friend
- **ESPHome.io**: component reference (and tease the ESPHome class)
- **Community**: ESPHome Discord, r/homeassistant, r/esphome
- **Quality YouTube**: digiblur and other rigorous folks
- **Influencer warning**: sponsorships/affiliate links, cross-reference the community
- **Content decay warning**: HA moves fast, check dates, go straight to official docs

## Q&A Stop 2 (10 min)

*Running time: ~140 min content + 10 break ≈ 2.5 hrs (likely faster).*

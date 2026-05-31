# Getting Started with Home Assistant

*Your take-home guide · Dallas Makerspace*

**What it is:** Home Assistant is the **middleman** that connects all your smart devices and protocols (Zigbee, Z-Wave, Wi-Fi, Bluetooth) into one local app — no ten vendor apps, no cloud lock-in, your data stays yours. It's free and open source, backed by the non-profit Open Home Foundation.

## Run It On

- **HA Green** — official plug-and-play box, easiest start (HAOS)
- **Raspberry Pi** — popular and well-documented
- **Mini PC or old laptop** — needs 6th-gen Intel or newer (a laptop works even with no screen). Lenovo M910q, or a new N100/N150 mini PC off Amazon.

## Protocols at a Glance

| Protocol | Good for | What you need |
| --- | --- | --- |
| **Z-Wave** | Very reliable, less congested. Great for door locks & critical sensors. | A controller — Home Assistant Connect ZWA-2 |
| **Zigbee** | Huge, cheap device selection. Self-healing mesh. | A coordinator — Home Assistant Connect ZBT-2 (ZHA or Zigbee2MQTT) |
| **Wi-Fi / Ethernet** | Simple, no hub. Cameras, Shelly relays, ESP devices. | Just your network |
| **Bluetooth / BLE** | Cheap room sensors, presence detection. | A BLE proxy (e.g. an ESP device) for range |
| **Thread / Matter** | The future — but still maturing (firmware/reliability). | Wait if you're just starting out |

## Bulbs vs. Switches vs. Relays

- **Smart bulbs** — per-bulb color & dimming, but if someone flips the wall switch the bulb loses power.
- **Smart switches** — replace the wall switch, keep normal bulbs, the wall still works for everyone.
- **In-wall relays (Shelly)** — hide behind your existing switch; best for retrofits and dumb fixtures.

**Rule of thumb:** switches & relays keep the wall working for family and guests; bulbs are for color everywhere.

## Gear Worth Buying

- **Z-Wave controller:** Home Assistant Connect ZWA-2 (the best, by far)
- **Zigbee coordinator:** Home Assistant Connect ZBT-2
- **Zigbee devices:** Philips Hue bulbs (pair straight to Zigbee2MQTT — no Hue bridge), Aqara (T1M, door/window sensors, Magic Cube), Third Reality
- **In-wall / Wi-Fi:** Shelly relays
- **LEDs:** WLED (on bulbs or LED controllers); a HUB75 matrix for DIY scoreboards
- **Voice:** Home Assistant Voice Preview Edition (local, but takes setup to rival Alexa/Google today)

## Integrations to Explore

UniFi & UniFi Protect (network presence + cameras) · WLED · Z-Wave · ESPHome · Alarmo (free alarm system) · BTHome (local Bluetooth sensors) · Frigate (local AI camera detection) · HomeKit Device (pair Wi-Fi devices via HomeKit) · HomeKit Bridge (push HA out to Siri) · Plex · Team Tracker (light effects when your team scores)

## Cool Things You Can Build

- **Automations** run themselves ("when X happens, do Y"). Example: mailbox opens → camera snapshot → phone notification with the photo → "You've got mail" plays on your speakers.
- **Leaving home** → lock doors, arm the alarm, turn off lights, sleep the PC — all automatically.
- **Scripts** are routines you trigger on demand — a "Wake Up" or "Goodnight" button, voice command, or dashboard tap.

## Go Further

- **ESPHome** — turn cheap ESP32 chips into your own HA sensors with simple YAML. (An ESPHome class is coming — watch for it.)
- **HACS** — the community store for custom dashboard cards, themes, and integrations.
- **Wall tablets + Fully Kiosk** — a cheap tablet as a wall dashboard that wakes on motion.
- **Claude Code + the HA MCP** — an AI agent that builds dashboards and automations for you, just by asking.

## Where to Learn More

- **Home Assistant** — <https://www.home-assistant.io>
- **ESPHome** — <https://esphome.io>
- **HACS** — <https://www.hacs.xyz>
- **Zigbee2MQTT** — <https://www.zigbee2mqtt.io>
- **WLED** — <https://kno.wled.ge> · **Frigate** — <https://frigate.video>
- **Shelly** — <https://www.shelly.com> · **Fully Kiosk** — <https://www.fully-kiosk.com>
- **Communities** — r/homeassistant, r/esphome, and the Home Assistant & ESPHome Discords
- **YouTube** — digiblur (technically rigorous). Watch out: many "influencers" push sponsored gear, and HA moves fast so old videos go stale — always check the date and the official docs.

## Support the Volunteers

Almost every integration talks to your devices through small open-source libraries (like **aiohttp**) that volunteers maintain for free. If HA saves you money, consider chipping in: <https://opencollective.com/aio-libs>

---

*Presented by Brandon · Apollo Automation · <https://apolloautomation.com>*

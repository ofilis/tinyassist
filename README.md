# TinyAssist

**TinyAssist** is a compact Home Assistant voice-assistant build based on the **M5Stack Atom Echo**, an **SSD1306 1.3-inch I²C OLED display**, and ESPHome's Home Assistant Voice Assist / wake-word stack.

The goal of this project is to turn an unused Atom Echo into a small desktop or workshop assistant with a custom display: it shows useful Home Assistant information while idle, then switches to clear visual states while the assistant is listening, processing, or responding.

<p float="left">
  <img src="photos/IMG_5684.jpeg" width="45%" alt="TinyAssist front view" />
  <img src="photos/IMG_5685.jpeg" width="45%" alt="TinyAssist assembled view" />
</p>

[Watch the project in action on Reddit.](https://www.reddit.com/r/Esphome/comments/1pbaipd/tinyassist/)

---

## What it does

TinyAssist combines voice-assistant functionality with a small information display.

When the assistant is idle, the OLED screen can show:

- Current time
- Full date and day name from Home Assistant
- Room temperature and humidity from Home Assistant sensors
- Door-lock state
- Weather condition icon

When the Home Assistant Assist satellite changes state, the display switches to assistant feedback screens:

- **LISTENING** — wake word / command is being captured
- **PROCESSING** — Home Assistant is processing the request
- **RESPONSE** — the assistant is responding

The included ESPHome YAML is based on the official M5Stack Atom Echo wake-word voice assistant package and extends it with I²C display output and Home Assistant sensor/text-sensor data.

---

## Hardware used

You will need:

- [M5Stack Atom Echo](https://shop.m5stack.com/products/atom-echo-smart-speaker-dev-kit)
- 1.3-inch SSD1306 I²C OLED display
- Terminal-style 3D printed enclosure  
  [Printables model](https://www.printables.com/model/160473-terminal-for-ssd1306-13-oled-and-wemos-d1-mini-new)
- Atom Echo frame / mount  
  [MakerWorld model](https://makerworld.com/tr/models/2063557-frame-for-m5stach-atom-echo#profileId-2228227)
- Slim 90-degree USB-C cable, or an alternative power path through the pins  
  [Example cable on AliExpress](https://aliexpress.com/item/1005009920122693.html?pdp_ext_f=%7B%22sku_id%22%3A%2212000050579928518%22%7D&sourceType=1&spm=a2g0o.wish-manage-home.0.0&gatewayAdapt=glo2tur)

> **Note:** The linked Atom Echo frame is my own model.

---

## Wiring

Connect the SSD1306 OLED display to the Atom Echo as follows:

| OLED display | M5Stack Atom Echo |
| --- | --- |
| VCC | 3.3V |
| GND | GND |
| SDA | GPIO25 |
| SCL | GPIO21 |

The ESPHome configuration uses the same pins:

```yaml
i2c:
  sda: GPIO25
  scl: GPIO21
```

---

## ESPHome configuration

The main configuration file is:

```text
m5stack-atom-echo.yaml
```

It imports the official ESPHome Atom Echo wake-word voice-assistant package:

```yaml
packages:
  m5stack.atom-echo-wake-word-voice-assistant: github://esphome/wake-word-voice-assistants/m5stack-atom-echo/m5stack-atom-echo.yaml@main
```

Then it adds:

- I²C display setup for the SSD1306 screen
- Home Assistant time sync
- Uptime timestamp sensor
- Home Assistant temperature and humidity sensors
- Home Assistant date/day text sensors
- Door-lock state
- Weather state and Material Design weather icons
- Display rendering logic for idle and voice-assistant states

### Things you may need to customize

Before flashing, review and adapt these parts of `m5stack-atom-echo.yaml` for your own Home Assistant installation:

- `substitutions.name`
- `substitutions.friendly_name`
- `api.encryption.key`
- Wi-Fi secrets
- Timezone
- Home Assistant entity IDs, especially:
  - Temperature sensor
  - Humidity sensor
  - Date/day sensors
  - Door lock
  - Weather entity
  - Assist satellite entity
- Font file paths and glyphs, depending on your ESPHome setup

The YAML references fonts such as:

```text
fonts/Google_Sans_Bold.ttf
fonts/Google_Sans_Medium.ttf
fonts/materialdesignicons-webfont.ttf
```

Make sure those font files exist in your ESPHome project directory, or replace them with your own fonts.

---

## Audio output note

The Atom Echo's built-in speaker is usable for simple feedback, but it is not very powerful. In my case, TinyAssist is a small workshop assistant rather than my main voice assistant, so the weak speaker is acceptable.

If you want better audio, you can route TTS output to another Home Assistant media player. This video explains one approach:

[YouTube video for using an external speaker](https://www.youtube.com/watch?v=o3yZWD_sFIE&t=390s)

You can also trigger another media player from ESPHome when TTS starts:

```yaml
voice_assistant:
  on_tts_start:
    - homeassistant.service:
        service: tts.speak
        data:
          # Set this to the entity ID of your OpenAI TTS or Piper TTS entity
          entity_id: tts.piper
          cache: "false"
          # Set this to your external media player
          media_player_entity_id: media_player.your_media_player
          message: !lambda 'return x;'
```

---

## Important power/cable note

Because of the shape of the original terminal-style 3D model, a standard USB-C cable does not fit well when powering the Atom Echo inside the enclosure.

You have two practical options:

1. Use a slim 90-degree USB-C cable.  
   [Example cable](https://aliexpress.com/item/1005009920122693.html?pdp_ext_f=%7B%22sku_id%22%3A%2212000050579928518%22%7D&sourceType=1&spm=a2g0o.wish-manage-home.0.0&gatewayAdapt=glo2tur)
2. Supply power through the pins.  
   This is the method I used in my build.

---

## Repository contents

```text
.
├── README.md
├── m5stack-atom-echo.yaml
└── photos/
    ├── IMG_5684.jpeg
    └── IMG_5685.jpeg
```

---

## Credits

- Built around the **M5Stack Atom Echo** hardware.
- Uses ESPHome's official wake-word voice-assistant package for Atom Echo.
- Enclosure is based on the linked terminal-style 3D model.
- Atom Echo frame/mount is my own 3D model.

---

## Disclaimer

This is a personal DIY Home Assistant / ESPHome project. The configuration is shared as a reference for similar builds. You will likely need to adjust entity IDs, fonts, timezone, secrets, and display content for your own Home Assistant setup.

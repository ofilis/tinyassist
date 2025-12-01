# TinyAssist

<p float="left">
  <img src="https://github.com/ofilis/tinyassist/blob/main/photos/IMG_5684.jpeg" width="45%" />
  <img src="https://github.com/ofilis/tinyassist/blob/main/photos/IMG_5685.jpeg" width="45%" />
</p>

This is TinyAssist. A mini voice assistant project based on an ESP32 and powered by Home Assistant Voice Assist.

**You will need the following:**
- M5Stack Atom Echo device
- Terminals 3D model. [You can download from here](https://www.printables.com/model/160473-terminal-for-ssd1306-13-oled-and-wemos-d1-mini-new)

I had an unused M5Stack Atom Echo device, so I decided to build a small voice assistant for my workshop with it. This is not my main assistant, so I don’t really care about the Echo’s weak speaker. But if you want, you can follow the steps in this video to route the audio to another device: 

[YouTUBE video for external speaker](https://www.youtube.com/watch?v=o3yZWD_sFIE&t=390s)

You can also make another media_player device perform TTS by adding the following line of code to your ESPHome YAML file.

```yaml
voice_assistant:
  on_tts_start:
    - homeassistant.service:
        service: tts.speak
        data:
          # Set this to the entity ID of your OpenAI TTS (or Piper TTS) entity
          entity_id: tts.piper
          cache: "false"
          # Set this to your external media player
          media_player_entity_id: media_player.your_media_player
          message: !lambda 'return x;'
```
**IMPORTANT NOTE**
“Due to the shape of the original terminal’s 3D model, it’s unfortunately not possible to power the Atom Echo with standard Type-C cables. Therefore, you either need to use an external 90-degree slim cable like the one in the link below, or supply power to the device through the pins (which is the method I chose).”

[Cable from AliExpress](https://tr.aliexpress.com/item/1005009920122693.html?pdp_ext_f=%7B%22sku_id%22%3A%2212000050579928518%22%7D&sourceType=1&spm=a2g0o.wish-manage-home.0.0&gatewayAdapt=glo2tur)

# TinyAssist

<p float="left">
  <img src="https://github.com/ofilis/tinyassist/blob/main/photos/IMG_5684.jpeg" width="45%" />
  <img src="https://github.com/ofilis/tinyassist/blob/main/photos/IMG_5685.jpeg" width="45%" />
</p>

This is TinyAssist. A mini voice assistant project based on an ESP32 and powered by Home Assistant Voice Assist.

I had an unused M5Stack Atom Echo device, so I decided to build a small voice assistant for my workshop with it. This is not my main assistant, so I don’t really care about the Echo’s weak speaker. But if you want, you can follow the steps in this video to route the audio to another device: 

https://www.youtube.com/watch?v=o3yZWD_sFIE&t=390s

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

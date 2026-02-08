# pytgvoip_pyrogram

[![PyPI](https://img.shields.io/pypi/v/pytgvoip_pyrogram.svg?style=flat)](https://pypi.org/project/stefano-pytgvoip-pyrogram/)

**Sample usage of [PytgVoIP](https://github.com/Sanji78/stefano-pytgvoip) library with [Pyrogram](https://github.com/Sanji78/stefano-pyrogram)**

```python
# making outgoing calls
from pyrogram import Client
from tgvoip_pyrogram import VoIPFileStreamService

app = Client('account')
app.start()

service = VoIPFileStreamService(app, receive_calls=False)
call = service.start_call('@bakatrouble')
call.play('input.raw')
call.play_on_hold(['input.raw'])
call.set_output_file('output.raw')

@call.on_call_ended
def call_ended(call):
    app.stop()
```

```python
# accepting incoming calls
from pyrogram import Client
from tgvoip_pyrogram import VoIPFileStreamService, VoIPIncomingFileStreamCall

app = Client('account')
app.start()

service = VoIPFileStreamService(app)

@service.on_incoming_call
def handle_call(call: VoIPIncomingFileStreamCall):
    call.accept()
    call.play('input.raw')
    call.play_on_hold(['input.raw'])
    call.set_output_file('output.raw')
    
    # you can use `call.on_call_ended(lambda _: app.stop())` here instead
    @call.on_call_ended
    def call_ended(call):
        app.stop()
```

[More examples](examples/README.md)

## Requirements
* Python 3.5 or higher
* PytgVoIP (listed as dependency)
* Pyrogram (listed as dependency)

## Installing
```pip3 install pytgvoip-pyrogram```


## Encoding audio streams
Streams consumed by `libtgvoip` should be encoded in 16-bit signed PCM audio.
```bash
$ ffmpeg -i input.mp3 -f s16le -ac 1 -ar 48000 -acodec pcm_s16le input.raw  # encode
$ ffmpeg -f s16le -ac 1 -ar 48000 -acodec pcm_s16le -i output.raw output.mp3  # decode
```

## ❤️ Donate
If this project helps you, consider buying me a coffee:  
**[PayPal](https://www.paypal.me/elenacapasso80)**.

..and yes... 😊 the paypal account is correct. Thank you so much!

---

## 📜 License
[MIT](LICENSE.md)


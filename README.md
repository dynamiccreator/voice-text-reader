# voice-text-reader
Realtime tts reading of large textfiles by your favourite voice. +Translation via LLM (Python script) 

# Description
This script reads any text file using voice cloning. It automatically splits the text into smaller chunks, creates wav files and plays them. If you stop the script and start it again with the same file it will start close to the same position where you have stopped it. The positions are stored in files ending on _pos.txt. You also can manually define a start position.

Additionally you can use a LLM via API to translate the text into a different language and read that translation instead. This all works in realtime, with a small lead time at the beginning on a 1050 GTX with just 4GB VRAM (It uses xtts-v2, and 4GB vram only works if you have closed anything else, so I recommend at least 6GB VRAM to be on the safe side)

# Installation

Make sure all required python packages are installed:

import os
import torch
from TTS.api import TTS #(coqui tts)
from pydub import AudioSegment
import simpleaudio as sa
import nltk
import threading
from queue import Queue
import string
import random
import openai
import re
import argparse

For real time usage you will need a NVIDIA GPU, at least 1050 GTX or better. So you must install cuda on your device.

# Usage
Make sure you have prepared your desired voice talking on a .wav or .mp3 file.

Reading an english text:
```
python voice-text-reader.py -t sample.txt -l en -sp desired_voice.mp3
```
Reading a german text starting at position of 1000 chars:
```
python voice-text-reader.py -t sample.txt -l de -sp desired_voice.mp3 -p 1000
```

Reading a text of any language translated to spanish:
```
python voice-text-reader.py -t sample.txt -l es -sp desired_voice.mp3 -p 1000 -trans spanish -trans_path http://localhost:1234 -trans_api API_KEY_HERE
```


```
All options:
  -h, --help            show this help message and exit
  -t TEXT, --text TEXT  The path of the text file to be read.
  -p POSITION, --position POSITION
                        The position in characters at which the reading of the file should start. Defaults to 0. If you do not set a value besides 0 the reading will continue
                        at the position where you have stopped the script.
  -l LANGUAGE, --language LANGUAGE
                        The language of the text. (en,de,fr,es....)
  -sp SPEAKER_FILE, --speaker_file SPEAKER_FILE
                        The path of the speaker file for voice cloning.
  -d DEVICE, --device DEVICE
                        The device for speak generation. cpu / cuda (default: cuda)
  -trans TRANSLATION, --translation TRANSLATION
                        The language the text is translated before it is converted into speech.(default: none) Should match language. But use the full english word like german
                        or italian not de or it as this is part of a prompt send to your LLM.
  -trans_path TRANSLATION_PATH, --translation_path TRANSLATION_PATH
                        The API path to the LLM model for translation. (e.g. http://localhost:1234)
  -trans_api TRANSLATION_API_KEY, --translation_api_key TRANSLATION_API_KEY
                        The API key for the LLM model used for translation.
```




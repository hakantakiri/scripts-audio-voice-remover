# Audio without voice using Demucs and ffmpeg

### Requirements:
- Demucs: Installed locally inside a virtual environment
- ffmpeg: Installed globally with brew in macos

### 1. Split the mp3 audio into separated tracks

```
python3 -m demucs -d cpu <path to mp3 file with extension> --mp3 -n mdx_extra_q
```
The --mp3 makes it work for .mp3 files, and the `-n mdx_extra_q` is optional for Demucs to use the older model. Depending on the audio you can achieve better results with or without it.


## 2. Mix te resulting tracks except for the voice
```
ffmpeg -i <audio1.mp3> -i <audio2.mp3> -i separated/<audio3.mp3> \
-filter_complex "[0:a][1:a][2:a]amix=inputs=3:duration=longest:dropout_transition=2[aout]" \
-map "[aout]" -c:a libmp3lame -b:a 320k  <out.mp3>
```
This command is set for 3 files, but you can eddit the inputs and _filter_complex for more ore less files.

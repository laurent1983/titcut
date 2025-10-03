# FFmpeg Commands Cheat Sheet for titcut

A quick reference for commonly used FFmpeg commands for **cutting, cropping, and recording videos**.

---

## Cut a video

```bash
ffmpeg -i input.mp4 -ss START -to END -c copy output.mp4
```
START → start time (e.g., 00:01:30)
END → end time (e.g., 00:02:45)
-c copy → copies streams without re-encoding (lossless & fast)

## Crop a video

```bash
ffmpeg -i input.mp4 -vf "crop=w:h:x:y" -c:a copy output.mp4
```

-c:a copy → keeps audio intact

## Screen record

```bash
ffmpeg -f pipewire -i 0 -c:v h264_qsv -preset veryfast -c:a aac -b:a 128k output.mp4
```

-f pipewire -i 0 → capture the full screen via PipeWire
-c:v h264_qsv → use Intel GPU hardware acceleration
-preset veryfast → fast encoding
-c:a aac -b:a 128k → audio encoding


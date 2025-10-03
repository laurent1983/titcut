# FFmpeg Commands Cheat Sheet for titcut

A quick reference for commonly used FFmpeg commands for **cutting, cropping, and recording videos**.

---

## Cut a video

```bash
ffmpeg -i input.mp4 -ss START -to END -c copy output.mp4
```
START → start time (e.g., 00:01:30)
END → end time (e.g., 00:02:45)
- -c copy → copies streams without re-encoding (lossless & fast)

## Crop a video

```bash
ffmpeg -i input.mp4 -vf "crop=w:h:x:y" -c:a copy output.mp4
```

- -c:a copy → keeps audio intact

## Video Info

```bash
ffprobe output.mp4 2>&1 | grep -E 'Duration|Stream'
```

## Screen record

### Basic recording (entire screen)

```bash
wf-recorder -f output.mp4
```

### Record specific area (interactive selection)

```bash
wf-recorder -g "$(slurp)" -f output.mp4
```

### Record with audio

```bash
wf-recorder -a -f output.mp4
```
- Stop recording with Ctrl+C

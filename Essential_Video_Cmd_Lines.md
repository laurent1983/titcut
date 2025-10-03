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


### Record using GPU

```bash
gpu-screen-recorder -w HDMI-A-1 -f 60 -a default_output -o test.mp4
```

### Record using GPU (specific area)

```bash
gpu-screen-recorder -w region -region "$(slurp -f '%wx%h+%x+%y')" -f 60 -a default_output -o output.mp4
```

## Compression to hd

```bash
ffmpeg -hwaccel vaapi -hwaccel_device /dev/dri/renderD128 -hwaccel_output_format vaapi \
-i output.mp4 -vf "scale_vaapi=-2:720" -c:v h264_vaapi -qp 23 -c:a copy output_720p.mp4
```
- -hwaccel vaapi: Enables VAAPI (Video Acceleration API) hardware acceleration, mainly used on Linux with Intel/AMD GPUs for faster decoding/encoding.
- -hwaccel_device /dev/dri/renderD128: Tells ffmpeg which GPU device to use.
On Linux, /dev/dri/renderD128 is usually the default render node.
- -hwaccel_output_format vaapi: Forces the intermediate video format to use VAAPI so the data stays on the GPU, avoiding unnecessary copies to RAM.
- -vf "scale_vaapi=-2:720": Scales the video using VAAPI. 720 → sets the height to 720 pixel. -2 → tells ffmpeg to automatically calculate the width while keeping the aspect ratio, but it must be divisible by 2 (codec requirement).
- -c:v h264_vaapi: Uses the hardware H.264 encoder via VAAPI for the output.
- -qp 23: Sets the quantizer parameter (quality).
- -c:a copy: Copies the audio stream without re-encoding (faster, no quality loss).




# METHODS TO EXTRACT FRAMES AS IMAGES OF A VIDEO USING FFMPEG

---

## ✅ Case 1: Extract frames as images (NO quality loss)

If your video is **24 FPS and 2 seconds**, that’s **48 frames**.

### 🔹 Exact command (lossless)

```bash
ffmpeg -i input.mp4 -vsync 0 frame_%03d.png
```

✔ No re-encoding
✔ Each frame is pixel-perfect
✔ PNG is lossless

You’ll get:

```
frame_001.png
frame_002.png
...
frame_048.png
```

---

## 🎯 Ensure exact frame count (important)

Some videos have **variable frame rate (VFR)**.

To force exact frame extraction:

```bash
ffmpeg -i input.mp4 -vf fps=24 frame_%03d.png
```

This guarantees **exactly 24 frames per second**.

---

## ✅ Case 2: Cut into 48 mini video clips (1 frame each)

Each clip = **1 frame video**
Still **no quality loss**.

### 🔹 Command

```bash
ffmpeg -i input.mp4 \
-vf fps=24 \
-c:v libx264 -crf 0 \
-f segment -segment_time 0.0416667 \
clip_%03d.mp4
```

📌 `0.0416667 = 1/24 second`

✔ Visually lossless
⚠️ Technically re-encoded (but CRF 0)

---

## ⭐ BEST METHOD (true zero-loss video clips)

If your video codec allows it (H.264/H.265):

```bash
ffmpeg -i input.mp4 \
-f segment \
-segment_frames 1 \
-reset_timestamps 1 \
-c copy \
clip_%03d.mp4
```

✔ **100% lossless**
✔ No encoding
✔ Ultra fast

⚠️ Works only if frames align with keyframes
⚠️ Some players may not like 1-frame videos

---

## 🧠 Recommended approach (practical)

| Goal               | Best Method    |
| ------------------ | -------------- |
| Exact frames       | PNG extraction |
| ML / AI processing | PNG / TIFF     |
| Re-edit later      | PNG            |
| One-frame videos   | segment_frames |
| Absolute zero loss | -c copy        |

---

## 🔑 Summary

✔ **Yes**, you can cut frames **without any quality loss**
✔ **Images = safest**
✔ **Video clips = possible but format-sensitive**

---

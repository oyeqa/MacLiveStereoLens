# Mac Live Stereo Lens for macOS and RayNeo Air Glasses

Mac Live Stereo Lens is a macOS app that converts a normal 2D desktop/video source into real-time Full-SBS stereoscopic output for AR video glasses such as **RayNeo Air 3** and **RayNeo Air 4**.

The recommended use case is watching video on a clean virtual 16:9 desktop and sending the stereoscopic result to the glasses.

## Recommended Setup

For the most stable and clean viewing experience, create a separate virtual display:

1. Create a **Virtual 16:9 1920x1080** display on macOS.
2. Move your video player, browser, or streaming window onto that virtual display.
3. Use that virtual display as the **Input Monitor** in Mac Live Stereo Lens.
4. Use your RayNeo glasses display as the **Output Monitor**.

This avoids capturing the MacBook built-in display, menu bar, desktop clutter, scaling changes, and accidental window movement. A dedicated 1920x1080 16:9 input also gives the stereo conversion a predictable image shape, which helps reduce jitter and distortion.

Recommended layout:

```text
Input Monitor:
  Virtual 16:9 1920x1080
  Put the video here and play it full screen or maximized.

Output Monitor:
  RayNeo Air / SmartGlasses display
  Usually 3840x1080 Full-SBS.
```

## What It Does

Mac Live Stereo Lens:

- Captures a selected macOS display using ScreenCaptureKit.
- Estimates depth in real time using a bundled CoreML conversion of **Depth Anything 3 Small (DA3-SMALL)**.
- Converts the input image into Full-SBS stereo.
- Outputs a 3840x1080-style stereo image suitable for RayNeo Air glasses and similar Full-SBS displays.
- Stabilizes depth during camera pans to reduce shimmer, pulsing, and micro-jumps.
- Locks 30 fps video cadence to smoother 60 Hz stereo output when possible.

## Requirements

- macOS 14 or later.
- Apple Silicon Mac recommended.
- RayNeo Air 3, RayNeo Air 4, or another display/glasses device that can show Full-SBS stereo.
- Screen Recording permission for the app.
- A virtual display tool if you want the recommended Virtual 16:9 1920x1080 workflow.

The app bundle includes the required CoreML model, so users do not need to install Python, PyTorch, or any developer tools.

## Depth Model

Mac Live Stereo Lens uses **Depth Anything 3 Small (DA3-SMALL)** for real-time monocular depth estimation.

The bundled model is packaged as:

```text
models/models--depth-anything--DA3-SMALL/model_378.mlpackage
```

The app runs this model locally through CoreML. It does not download a model at runtime and does not depend on any separate app installation.

## Local Files

Mac Live Stereo Lens is intended to run as a normal standalone `.app`.

- It does not install background services, helper tools, drivers, or extra packages.
- It does not create an app log file during normal use.
- It saves the selected displays and IPD after a successful manual start.
- It keeps the compiled CoreML model in `~/Library/Caches/com.oyeqa.maclivestereolens/CoreML` so repeat launches do not waste time and power recompiling it.
- Temporary run files are removed when the app exits.

If you redistribute this project or use it commercially, review the upstream Depth Anything 3 / DA3-SMALL license and attribution requirements.

## First Launch

Because this app is distributed outside the Mac App Store, macOS may block it on first launch.

If macOS says the app cannot be opened:

1. Right-click `MacLiveStereoLens.app`.
2. Choose **Open**.
3. Confirm that you want to open it.

Then grant Screen Recording permission:

1. Open **System Settings**.
2. Go to **Privacy & Security**.
3. Open **Screen & System Audio Recording**.
4. Enable permission for Mac Live Stereo Lens.
5. Quit and reopen the app if macOS asks you to.

## Basic Usage

1. Connect your RayNeo Air glasses to your Mac.
2. Create or enable a **Virtual 16:9 1920x1080** display.
3. Put your video on the virtual display.
4. Open `MacLiveStereoLens.app`.
5. Select the virtual display as **Input Monitor**.
6. Select the RayNeo glasses display as **Output Monitor**.
7. Adjust **IPD** if the stereo separation feels uncomfortable.
8. Press **Start**.

Only IPD is exposed as a normal user control. The depth and stabilization settings are intentionally fixed because they are easy to over-tune and can cause jitter, edge distortion, or uncomfortable stereo.

**Auto Start** is off by default for first-time users. After you select displays and press **Start** successfully, the app automatically saves the current display choices and IPD to your user settings. If you enable Auto Start yourself, that choice is saved too.

## IPD

IPD means interpupillary distance, or the distance between your eyes. Different people may need slightly different values.

Use small changes:

- Lower IPD if the stereo effect feels too strong or causes eye strain.
- Higher IPD if the stereo effect feels too flat.
- If you are unsure, start with the default value.

## Tips For Best Results

- Use a dedicated **Virtual 16:9 1920x1080** input display whenever possible.
- Put only the video content on that virtual display.
- Avoid changing macOS display arrangement while the app is running.
- Prefer full-screen or maximized playback on the virtual display.
- For 30 fps video, keep the app output at 60 fps.
- Do not use the MacBook built-in display as both input and output unless you are only testing.
- If the image feels unstable, stop and start the app after changing display arrangement.

## Troubleshooting

### The output is black

- Check that the selected Output Monitor is the RayNeo/SmartGlasses display.
- Check that the glasses are connected and visible in macOS Display settings.
- Stop and Start again after connecting or rearranging displays.
- Make sure the input display actually contains visible video content.

### The wrong display is captured

- Open Mac Live Stereo Lens again and check **Input Monitor**.
- Prefer a display named like `Virtual 16:9 1920x1080`.
- Do not rely on display IDs; macOS can change display IDs after reconnects or display arrangement changes.

### The stereo image feels uncomfortable

- Adjust only **IPD**.
- Lower IPD if the stereo effect is too strong.
- Take a short break if you feel eye strain.

### The image jitters during camera pans

The app includes cadence locking and depth stabilization for panning scenes. For best results:

- Use a clean Virtual 16:9 1920x1080 input display.
- Keep the video player on that display.
- Avoid dragging windows or changing monitor layout during playback.
- Restart the app after major display changes.

## Notes For GitHub Distribution

This project is intended to be distributed as an app bundle for normal users. Users should not need to build from source.

Recommended release format:

- Upload `MacLiveStereoLens.app` as the main app artifact.
- If GitHub requires a single file upload, compress the `.app` bundle for transport, but the user-facing artifact should still be the macOS app.
- Include this README text in the GitHub repository or GitHub Release description.
- Clearly mention that the bundled depth model is **Depth Anything 3 Small (DA3-SMALL)**.

The app is ad-hoc signed for local distribution. It is not notarized unless the release maintainer signs and notarizes it with an Apple Developer ID.

## Known Limitations

- This is real-time monocular depth conversion, not native 3D content.
- Some scenes with fast cuts, transparent objects, subtitles, or complex foreground edges may show artifacts.
- Very slow camera pans can reveal tiny cadence or depth changes more clearly than normal playback.
- Power usage depends on display capture, CoreML depth estimation, and output resolution.

## Recommended Workflow Summary

```text
1. Connect RayNeo Air glasses.
2. Create Virtual 16:9 1920x1080.
3. Put the video on that virtual display.
4. Open Mac Live Stereo Lens.
5. Input Monitor  = Virtual 16:9 1920x1080.
6. Output Monitor = RayNeo / SmartGlasses display.
7. Adjust IPD only if needed.
8. Press Start.
```

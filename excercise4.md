# Video Transcoder

This document provides a detailed breakdown of the video transcoder's aspect ratio detection and preset selection function.

## Function Overview

The function is designed to determine the aspect ratio of a video and, based on that aspect ratio, filter and return the appropriate presets from a given configuration.

### Parameters

- `config`: A dictionary containing presets.
- `w`: Width of the video.
- `h`: Height of the video.

## Code Snippet

```python
def fn(config, w, h):
    ar = w / h

    if ar < 1:
        return [r for r in config['p'] if r['width'] <= w]
    elif ar > 4 / 3:
        return [r for r in config['l'] if r['width'] <= w]
    else:
        return [r for r in config['s'] if r['width'] <= w]
```

## Aspect Ratio

The aspect ratio (`ar`) is calculated using the formula:

```
ar = w / h
```

The aspect ratio determines the proportional relationship between the video's width and height. It's a measure to tell how "wide" or "tall" the video appears.

## Preset Selection Logic

1. **Portrait Mode** (`ar < 1`): 
    - If the video is taller than it is wide (a portrait mode video).
    - Filters presets available under `config['p']` to include those where the preset width (`r['width']`) is less than or equal to the video width (`w`).

2. **Widescreen/Landscape Mode** (`ar > 4 / 3`):
    - If the video is wider, suggesting a widescreen or landscape format.
    - Filters presets available under `config['l']` to include those where the preset width (`r['width']`) is less than or equal to the video width (`w`).

3. **Standard Format** (else):
    - If the video has an aspect ratio close to 4:3, suggesting a standard video format.
    - Filters presets available under `config['s']` to include those where the preset width (`r['width']`) is less than or equal to the video width (`w`).

## Return Value

The function returns a list of suitable presets based on the video's aspect ratio and width.

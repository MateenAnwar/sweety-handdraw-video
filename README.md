# sweety-handdraw-video · Topic Hand-Drawn Video

**English** · [中文](README_zh.md)

> Fork of [threerocks/sweety-handdraw-video](https://github.com/threerocks/sweety-handdraw-video). All credit for the project goes to the original author.

Turn a topic, an article, or an explainer script into an explainer video with narration, a cover, and illustrations drawn stroke by stroke.

**The model decides what to explain and what to draw; local tools decide how to draw it, when to draw it, and how to composite it.** Every video is designed from scratch for its content. There is no fixed number of pages, duration, or characters.

```bash
npx skills add MateenAnwar/sweety-handdraw-video --skill sweety-handdraw-video
```

[Watch the sample](https://github.com/MateenAnwar/sweety-handdraw-video/releases/tag/v1.0.0) · [Download the package](https://github.com/MateenAnwar/sweety-handdraw-video/releases/download/v1.0.0/sweety-handdraw-video-v1.0.0.zip) · [Production rules](skills/sweety-handdraw-video/SKILL.md) · [Validation scope](docs/validation.md)

## See it in action

### "Murphy's Law"

About 2 min 05 s, with 8 scenes and 24 narration segments. It opens with everyday mishaps, like forgetting an umbrella or a file that won't open. Then it explains how preparation lowers the cost of mistakes, and ends with checklists, backups, and doing a trial run in advance. The sample uses a light watercolor-sketch style, a female hand with long rhinestone nails, custom text on the pen barrel, and the Iapetus reference voice.

[![Murphy's Law cover](docs/media/murphy-cover.jpg)](https://github.com/MateenAnwar/sweety-handdraw-video/releases/download/v1.0.0/murphy-law.mp4)

![Line drawing, coloring, and text](docs/media/murphy-drawing.gif)

The GIF is a silent loop taken from the actual exported video. [Download the full sample with audio](https://github.com/MateenAnwar/sweety-handdraw-video/releases/download/v1.0.0/murphy-law.mp4).

![Final page](docs/media/murphy-final.jpg)

The characters, layout, orange color scheme, and ad names in the sample are not a default template. A new topic gets a new design. When you revise the same video, images and voice-over you have already approved can be reused.

## You ask, it makes

| Your request | How it's made |
|---|---|
| "Explain sunk cost with a hand-drawn video" | Picks an explanation path, designs everyday scenes and cause-and-effect, then generates narration, visuals, and the final video |
| "Turn this article into an explainer video" | Keeps the article's facts and qualifiers first, then rearranges them into short segments that work for listening and watching |
| "Switch to a female hand and print my name on the pen" | Uses a calibrated hand preset, places the given text on the pen barrel, and checks it is readable at normal size |
| "The text on this page got cut off, redo it" | Fixes the affected text layer and layout, keeps images and voice-over that already passed, then re-renders and re-checks |

## Install a single Skill

All five production dependencies come bundled, so you don't need to install them separately:

| Bundled dependency | Purpose |
|---|---|
| sweety-image-art-direction | Art direction for each image: subject, contact, composition, proportion, and visual hierarchy |
| hand-drawn-styles | Complete recipes and reference anchors for hand-drawn styles |
| omniailab-ai-director | Directorial planning, aesthetics, audio-visual relationships, and final review |
| huashu-art-motion | Applies its methods when the topic calls for art-motion effects |
| sweety-image-privacy | Strips privacy metadata from generated images and records what remains |

Dependencies are bundled as pinned-version snapshots. Each dependency guide is named `GUIDE.md`, and the whole install folder has only one `SKILL.md`. The package does not register five extra Skills and does not silently update upstream code at runtime. Sources, licenses, modifications, and hashes are listed in the [dependency notes](skills/sweety-handdraw-video/references/dependencies.md) and the [source list](THIRD_PARTY_NOTICES.md).

The bundle includes production materials and runtime code. **You still need the following software or capabilities:**

- **VoiceStudio**: generates, edits, and exports narration. If it isn't installed or running, no new narration can be generated. Previously checked, exported audio can still be composited offline. The tool never switches voice providers automatically.
- **Image generation**: Codex's built-in image generation by default. Every new image must pass art direction first.
- **Python 3.9+, FFmpeg/ffprobe, and Chinese fonts**: used for line drawing, layout, and compositing. Installing `scripts/requirements.txt` in a per-task virtual environment is recommended.
- **Bun and ExifTool**: run the bundled image-privacy cleanup. The pipeline removes the fake camera fields that the original cleanup script writes.

The current VoiceStudio adapter targets a local HTTP API, by default `http://127.0.0.1:3900`, and supports `mlx-audio / Qwen3-TTS-12Hz-1.7B-Base-bf16`. It needs compatible `/engines/tts` and `/generate` endpoints, plus an optional `/transcribe` endpoint. Not every program with the same name is compatible; see the [integration requirements](skills/sweety-handdraw-video/references/local-runtime.md). The tool does not download models automatically or change your global engine.

To install manually, copy the **entire** `skills/sweety-handdraw-video` folder into your tool's personal Skills directory, for example `~/.codex/skills/sweety-handdraw-video` for Codex. Back up any existing folder before updating, and don't copy only the entry file.

## Getting started

After installing, send your request to the agent:

```text
Use $sweety-handdraw-video to explain "Murphy's Law" to an ordinary adult audience.
Start from everyday frustrations, then explain the mechanism and practical ways to prepare.
```

```text
Use $sweety-handdraw-video to turn the article below into a video, keeping the important conditions and data.
Use a female arm and print @leo_xiaolei on the pen barrel.
```

Defaults when not specified: Simplified Chinese, 1920×1080, 30 fps, the navy-sleeve marker hand, and the Iapetus reference voice. Video length depends on the explanation and the actual voice-over. There is a fixed 1-second cover: it holds still for the first 0.5 s and fades out over the next 0.5 s, and then the main content and narration begin.

### Six hands, five voices

![Six hand presets](docs/media/hand-presets.jpg)

Hands: navy-sleeve marker, female soft-pink long rhinestone nails, child with colored pencil, brush inspired by Shangguan Wan'er, white-shirt gel pen, and knit-sleeve fountain pen. Custom text on the pen barrel is supported, and the cut edge of the forearm stays out of frame.

Voices: Iapetus (clear narration), sweet senior schoolmate, friendly male lecturer, intellectual female lecturer, and warm male narrator. All are synthetic reference voices. Iapetus is synthesized in VoiceStudio from reference audio. **It does not call Gemini's native TTS directly.** Voice names and descriptions are only a guide to choosing; they don't replace actually listening.

## How it's made and checked

The model first decides which narrative and visuals fit the content. For common everyday concepts it starts by evaluating "mistake or frustration → concept → mechanism → action → wrap-up". If that doesn't fit, it plans again instead of forcing the structure.

For illustrations, the model looks at the image and labels its semantic parts. It draws all the outlines in order, then fills in color in the same order. Each character of text is isolated, and the pen lifts between strokes. Color appears only where the brush has passed. Glyphs are laid out by their real bounds with margin on all four sides, so the bottoms of Chinese characters and English descenders don't get clipped.

The voice-over drives the timeline. Representative clips are checked first, then the full video is exported. Automated checks cover timing, coverage, canvas bounds, subtitles, and audio/video integrity. The model also reviews the final video for content, semantic regions, occlusion, and reading pace. Subjective listening quality, emotion, and learning effectiveness are recorded separately, and nothing is marked "passed" without being checked.

Final deliverables are an MP4, a cover PNG, an SRT, the narration script, and the check records. `finalize` refuses to proceed if a check is missing, if old records are reused after assets changed, or if a test fixture is passed off as a finished video.

## Development and validation

```bash
python3 -m venv .venv
source .venv/bin/activate
python -m pip install -r skills/sweety-handdraw-video/scripts/requirements.txt
python skills/sweety-handdraw-video/scripts/validate_package.py
python skills/sweety-handdraw-video/scripts/test_drawing_contract.py
python skills/sweety-handdraw-video/scripts/smoke_test.py --output /tmp/handdraw-smoke-new
```

The last command creates a new test directory and needs FFmpeg and Chinese fonts. It checks runtime behavior; it does not judge how the finished video looks. To build the install package, run `python3 tools/package.py --output dist`.

The full pipeline has been validated on macOS. Linux CI has passed the dependency integrity, glyph and drawing regression, short-video compositing, and packaging checks. Windows and other VoiceStudio backends have not been validated end to end. Glyph tracing doesn't guarantee standard calligraphic stroke order, and any new topic still needs to be checked by hand.

## Acknowledgements and license

The main project code and docs are MIT licensed. Bundled dependencies keep their own copyrights and licenses; see [THIRD_PARTY_NOTICES.md](THIRD_PARTY_NOTICES.md). Thanks to Huashu Art Motion, OmniAiLab AI Director, the hand-drawn style library, and the art direction project for their methods. The AI-generated hand assets and synthetic reference audio have their sources documented separately. The project does not claim any real person's identity, official character licensing, or a general guarantee of output quality.

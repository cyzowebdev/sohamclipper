PROMPT ----------- 

# Fresh-computer setup prompt — Soham's Clipper

GITHUB_REPO_URL = (https://github.com/cyzowebdev/sohamclipper)

---

You are setting up and verifying Soham's Clipper on this computer from the GitHub repository above. Do the whole job end-to-end. Use the repository contents as the source of truth. Do not ask me for details that are already in this prompt.

## 1. Repository and destination

1. Confirm GITHUB_REPO_URL is set and is not still the placeholder. If it is still the placeholder, stop and tell me exactly which line to fill in.
2. Make sure git is installed. If it is missing, install it or give me the exact install command for this OS and stop.
3. Clone into a short, permanent folder:
   - Windows: C:\sohamclipper
   - macOS/Linux: ~/sohamclipper
4. Do not clone into a deep temporary/assistant workspace. If the chosen path is temporary or inside another workspace, ask me before moving it.
5. The repository is the updated Soham's Clipper source package. It should contain studio/, web/, tests/, tools/, deploy/, requirements.txt, .env.example, README.md, AGENTS.md, start-sohamclipper.bat, and deploy/nginx-sohamclipper.conf. If any of these are missing, report it before continuing.

## 2. Prerequisites

1. Use Python 3.12 if available; Python 3.11+ is acceptable. Check python --version / python3 --version.
2. Check that ffmpeg and ffprobe are installed and on PATH with ffmpeg -version and ffprobe -version.
3. If they are missing, install them for this OS:
   - Windows: winget install Gyan.FFmpeg
   - macOS: brew install ffmpeg
   - Debian/Ubuntu: sudo apt install ffmpeg
4. If automatic installation is not possible, give me the exact command and stop.
5. Do not install or run anything with a real YouTube upload. Never post or upload during verification.

## 3. Python environment

From the cloned project folder:

1. Create a virtual environment named .venv.
2. Upgrade pip inside it.
3. Install requirements.txt.
4. Use the OS-specific Python path for all later commands:
   - Windows: .venv\Scripts\python.exe
   - macOS/Linux: .venv/bin/python

Do not install the project globally. Do not modify requirements.txt unless an install fails; if it fails, report the exact error and proposed fix first.

## 4. Configuration

1. Do not create or commit a .env file unless it is genuinely needed. .env.example is the template.
2. No AI key is required. If no key is configured, the app must fall back to loudness-only clip selection and still work.
3. If I provide an AI key later, put it only in the local .env as SOHAMCLIPPER_AI_KEY, or configure it through the dashboard under Settings → AI. Never print the key back to me.
4. The Instagram follow button must remain enabled and point to https://www.instagram.com/sohamdv/. This is the default value of SOHAMCLIPPER_FOLLOW_URL. An empty value hides it.
5. The visible display name must stay exactly Soham's Clipper. Machine identifiers stay sohamclipper, SOHAMCLIPPER_, sohamclipper_.

## 5. Run the app

1. Start the server with python -m studio.server using the .venv interpreter.
2. Confirm http://127.0.0.1:8765/healthz returns OK.
3. Open http://127.0.0.1:8765/ and then http://127.0.0.1:8765/app.
4. If port 8765 is taken, set SOHAMCLIPPER_PORT to a free port in the local environment and tell me the final URL. Do not change committed defaults.

## 6. Verification you must perform

### Full test suite

From the project root, run the complete suite:

- Windows:
  set PYTHONIOENCODING=utf-8 && .venv\Scripts\python.exe tests\run.py
- macOS/Linux:
  PYTHONIOENCODING=utf-8 .venv/bin/python tests/run.py

Run only one suite at a time. Overlapping runs can collide on shared test data and port 8765. All tests must pass. If any fail, diagnose and fix the cause, then rerun once cleanly.

### Browser checks

Check both Home and All videos:

1. Page title is Soham's Clipper.
2. The top-right Follow me Instagram button is visible.
3. Its link is exactly https://www.instagram.com/sohamdv/, opens in a new tab, and has safe rel attributes.
4. There are no Star on GitHub, Send feedback, See every change on GitHub, Report, or Report this buttons/popups.
5. No browser console errors.

Also verify the clip editor:

1. Open any existing clip's Edit button, or create a test clip locally if needed.
2. The source video loads and the live preview renders from the real renderer.
3. Typing in the heading/caption fields and dragging the HEADING handle updates the preview.
4. Apply re-renders through the real render worker and the updated clip appears in All videos.
5. Split labels match outputs exactly:
   - face_top = facecam top, gameplay bottom
   - face_bottom = gameplay top, facecam bottom
   - face_left = facecam left, gameplay right
   - face_right = gameplay left, facecam right
   Only the two selected regions appear in the output.

### Final source scan

Scan the repository text files. The only allowed remaining peakcut strings are:

- https://github.com/haziqahmedqasmi/peakcut
- https://huggingface.co/madwezza/peakcut-arabic-speech/resolve/...

There must be no remaining SOHAMS_CLIPPER_, sohams_clipper_, sohams-clipper, SohamsClipper, sohamsclipper, C:\peakcut, C:/peakcut, or PeakCut strings. Runtime data/ and .venv/ are expected to be absent from a fresh clone and do not count as source files.

## 7. Feature expectations to preserve

Do not simplify away these behaviors:

- Live clipping watches chat hype without downloading or recording until the user presses Clip.
- Pressing Clip fetches only the requested past 5–60 seconds, never the whole stream.
- Multiple live clips can run concurrently; no busy lock.
- Active watching survives browser refresh while the server keeps running.
- Each live clip becomes a normal job in All videos.
- Live captions default off because transcription is slow; they can be enabled explicitly.
- Hindi/Hinglish speech model whisper-hinglish-sherpa-onnx-int8 is supported, along with the existing speech loader fix for joiner*.onnx model folders.
- Functional social-account features stay: YouTube/Instagram/TikTok OAuth connect, View on YouTube, Twitch watch links, developer registration links, and API-key provider URLs.
- Legal pages and the legal Reporting section stay; only promo/report pop-ups and buttons were removed.

## 8. What to report back

When finished, report:

1. Final install folder.
2. Python and ffmpeg/ffprobe versions.
3. Final app URL.
4. Full test-suite result.
5. Browser-check result.
6. Any deviations, fixes made, or remaining issues.
7. Confirmation that no YouTube upload was performed.

# Microsoft Edge: Autoplay Bypass and Audio Persistence via Sandboxed Iframe

This repository contains a Proof of Concept (PoC) for a security issue I found in Microsoft Edge (Chromium-based). The PoC demonstrates how audio can be triggered without a valid user gesture and how it persists after a soft navigation.

## What I Found

While testing Edge's behavior with audio APIs inside a sandboxed iframe, I noticed two things:

1.  **Autoplay Bypass**: Audio (a simple tone and TTS) can be played automatically when the page loads, without any interaction from the user. This goes against the default Autoplay Policy that should block such behavior.
2.  **Audio Persistence**: After the audio starts, I use `history.pushState` to change the URL and replace the page content with a fake Google login form. The audio keeps playing in the background, even though the page has visually changed and the URL is different.

## Steps to Reproduce

1.  Open the PoC URL in Microsoft Edge (I tested on version 156.0.4314.58 on macOS):
    `https://drajatakbarr.github.io/zero-click-audio-poc/index.html`
2.  **Do not interact with the page** (no clicks, no mouse movement, no scrolling).
3.  You should hear a tone and a TTS message ("Security Alert...").
4.  After about 1.5 seconds, the URL will change to `/fake-google-login` and the page will display a fake Google login form.
5.  The audio continues to play in the background, even if you open a new tab.

## Why This Matters

If an attacker can trigger audio without user interaction and keep it playing while the page is spoofed, it could be used for social engineering. For example, a victim might hear a fake security warning while seeing a fake login page, which could trick them into entering credentials.

I am not claiming this is a full exploit chain or a memory corruption bug. It is a logic/behavior issue in how Edge handles audio inside iframes and during soft navigation.

## Environment

*   **Browser**: Microsoft Edge (Chromium) - Version 156.0.4314.58
*   **OS**: macOS 26.6.2 (Intel x86_64)
*   **Tested on**: A fresh Edge profile with default settings.

## Disclaimer

This PoC is for educational and security research purposes only. Do not use it for malicious activities.

# Documentation: LiveSessionPlayer.jsx

## Overview
`LiveSessionPlayer.jsx` is a specialized video player designed for real-time attendance and synchronized learning.

## Detailed Explanation
Unlike the regular course player, the Live Player is built to simulate a "Broadcast" experience. It has a high degree of control over the video playback:
- **Countdown**: If a student joins early, they see a beautiful countdown timer (`10h 5m 20s`).
- **Live Sync**: If a student joins 10 minutes late, the video automatically jumps 10 minutes ahead.
- **Cheat Prevention**: During a "Live" session, the player disables fast-forwarding and enforces a `1.0x` playback speed.

## Program Flow
1. **The Pulse**: A `setInterval` runs every second to update the `status` (upcoming/active/ended) and the `timeLeft` countdown.
2. **Seek Correction**: Uses the `onSeeking` and `onTimeUpdate` events to detect if a student is trying to "Seek" past the current live point. If they do, the player forcibly moves them back to the correct timestamp.
3. **Playback Rate Enforcement**: Uses `onRateChange` to detect if a student tries to speed up the video (e.g., 2.0x). It immediately resets it to `1.0x`.

## Why it is used
To ensure "Synchronicity". In a live classroom, everyone should be seeing the same content at the same time. This player enforces that rule programmatically.

## Interview Questions
1. **How do you calculate the "Live Point" of a video if a student joins late?**
   *Answer: Time Differentials. We subtract the `startTime` from the current time (`new Date()`). We move the video's `currentTime` to exactly that number of seconds. If the video is 30 minutes long and 10 minutes have passed, we start at 10m 0s.*
2. **Why do you use `setInterval` instead of just a single timeout?**
   *Answer: Real-time UI. We need the countdown clock to tick every second. `setInterval` allows us to recalculate the `timeLeft` string continuously until the session goes live.*
3. **Explain how you prevent users from bypassing the "Live" restriction.**
   *Answer: By combining multiple events. We use `onSeeking` (to block manual scrubbing), `onTimeUpdate` (to block "Skipping" through the keyboard), and `onRateChange` (to block playback speed hacks). This creates a "Three-Layer" defense.*
  

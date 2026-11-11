# Pitch 432Hz (Magisk module)

Retunes system audio from A=440 Hz to A=432 Hz (a drop of about 31.77 cents) using a SoundTouch pitch-shift effect. Tempo is not changed.

## What's inside

- `libpitch432.so`: native audio effect (effect API 2.0), pitch ratio 432/440 = 0.9818, installed to `soundfx` for 64-bit and 32-bit.
- `Pitch432.apk` (`com.jama.pitch432`): privileged system app with no launcher and no UI. It attaches the effect to the global music output at boot and keeps it attached.
- Patched `audio_effects.xml` files that register the effect.
- Privapp permissions allowlist for `MODIFY_DEFAULT_AUDIO_EFFECTS`.

Reboot twice after flashing so the priv-app gets registered.

## FAQ

**Can the app be killed?**
It is `persistent`, so the low-memory killer and swiping recents don't touch it. If it is killed, the system restarts it. If audioserver restarts, the app re-attaches the effect within about 5 seconds. While the app is dead, audio plays at 440 Hz.
A Force Stop from Settings or `am force-stop` can keep it down until the next reboot. HyperOS autostart/battery managers can also interfere, so whitelist `com.jama.pitch432` and set battery to "No restrictions".

**Does it poll the CPU constantly? Does deep sleep kill it?**
The app checks every 5 s (15 s while failing) with two tiny binder calls. It holds no wakelock and never wakes the device. The timer pauses during suspend and resumes on wake. Doze and deep sleep don't kill persistent apps. The real CPU cost is SoundTouch, and it only runs while audio is playing.

**Does it conflict with Dolby or other effects?**
No. Dolby and other effects change tone and loudness, not pitch. Per-app effects run first, then this global effect runs last on the mix. You don't need to turn anything off. Each extra effect adds some latency and CPU.

**Is everything always 432 Hz?**
Only audio that goes through the normal mixer on the music output. These bypass it:
- Low-latency / fast-mixer streams (many games)
- Offloaded playback (many music and video apps)
- Direct outputs (USB DAC passthrough, some Bluetooth codec paths)
- Non-music streams (calls, alarms)

## 440 Hz vs 432 Hz: facts

**Background**
- A4 = 440 Hz became the international standard (ISO 16) in 1955, after a 1939 London conference. Before that, pitch varied widely by era and region, roughly 415 to 450+ Hz.
- Many orchestras today tune to 442-443 Hz for brightness.
- Baroque ensembles often use 415 Hz; the classical period was around 430 Hz.

**Common claims about 432 Hz**
- Calmer, more "natural", "in harmony with the universe"
- Linked to the Schumann resonance, Verdi's pitch, or sacred geometry
- 440 Hz was imposed as a form of control

**What the evidence says**
- A few small studies reported slightly lower heart rate or a preference for 432 Hz. They are tiny, poorly controlled and not replicated.
- Nothing about 432 Hz is mathematically special. Its "purity" claim rests on C = 256 Hz being a tidy number, which only matters in a C-based system, and equal temperament doesn't give clean ratios anyway.
- The Schumann resonance is about 7.83 Hz and is unrelated.
- The story that 440 Hz was a Nazi/Goebbels plot has no real historical backing. The standard came from international conferences.
- Any perceived difference is most likely expectation (placebo), or the fact that a slightly lower pitch can sound a bit warmer.

**Practical numbers**
- 440 -> 432 = ratio 0.9818, about -31.77 cents.
- Shifting by resampling would also slow tempo by about 1.8%. This module shifts pitch only, so tempo stays the same.
- Pitch shifting can add small artifacts. Playing along with other instruments tuned to 440 will sound out of tune.

**Bottom line:** 432 Hz is a fine personal taste, but not a health or spiritual upgrade.

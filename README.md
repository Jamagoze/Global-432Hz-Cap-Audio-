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

## Version history

**v1.4** (current)
- Fixed the effect not loading on Android 14 / HyperOS: the effect library header now reports library API version 3.0, which this device's audio HAL requires (version 2.0 was loaded and then rejected). The effect descriptor stays at control API 2.0, same as the stock effects.

**v1.3**
- Debug build. Fixed the `magiskpolicy` syntax for creating the log folder's SELinux type (it accepts only one attribute per `type` statement).

**v1.2**
- Debug build. SELinux rules applied live with `magiskpolicy` at boot, because `sepolicy.rule` was not taking effect on this device.

**v1.1**
- Debug build. Added a file log (`/data/pitch432/pitch432.log`) with `config.txt` switches for log on/off and max log size (1 MB), because logcat was silenced on this ROM.

**v1.0**
- First release: SoundTouch pitch-shift effect (A=440 -> 432 Hz, tempo unchanged), UI-less privileged app that attaches it to the global music output, patched `audio_effects.xml` files, privapp permission allowlist.

## Checking that it's active

```sh
dumpsys media.audio_flinger | grep -B2 -A6 "NOT loaded"      # pitch432 must not be listed here
dumpsys media.audio_flinger | grep -A8 "Library pitch432"    # library registered
dumpsys media.audio_flinger | grep -B12 "name: Pitch432"     # effect on session 0, Enabled = y
```

If logcat is empty, a module that disables logging is probably installed; turn it off and reboot to see effect loader messages (`logcat | grep -iE "EffectFactoryHAL|pitch"`).

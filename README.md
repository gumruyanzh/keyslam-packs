# Keyslam sound packs

The open file format for sound packs used by [Keyslam](https://keyslam.com), the Mac keyboard sound app that plays real mechanical keyboard and typewriter recordings as you type. On MacBooks with Apple's motion sensor it picks soft, medium, hard or slam strikes to match how hard you type.

This repo holds the format spec, a JSON Schema and a complete example pack. Keyslam itself is launching soon: see [keyslam.com](https://keyslam.com), and try every bundled sound in the browser at [keyslam.com/keyboard-simulator/](https://keyslam.com/keyboard-simulator/).

## Install a pack

1. In Keyslam's menu bar menu, choose **Packs Folder…** (it opens `~/Library/Application Support/Keyslam/Packs`).
2. Copy the pack's folder (the one containing `pack.json`) into it.
3. Open the Keyslam menu again. Packs are rescanned every time the menu opens.

## Folder layout

```
my-pack/
  pack.json
  key-soft-1.wav  key-medium-1.wav  key-hard-1.wav  key-slam-1.wav  ...
  space-medium-1.wav  enter-medium-1.wav  ...
  key-up-1.wav  ...
```

Audio files can be WAV, AIFF or FLAC (anything macOS's `AVAudioFile` opens). Short, tightly trimmed takes with no silence before the strike sound best: Keyslam starts the sound the moment the key goes down.

## pack.json

```json
{
  "formatVersion": 1,
  "id": "my-pack",
  "name": "My Pack",
  "kind": "Tactile",
  "summary": "Optional one-line description",
  "author": "Who recorded it",
  "license": "CC0 1.0",
  "groups": {
    "alpha": { "down": { "soft": ["key-soft-1.wav"], "medium": ["key-medium-1.wav"], "hard": ["key-hard-1.wav"], "slam": ["key-slam-1.wav"] }, "up": ["key-up-1.wav"] },
    "space": { "down": { "medium": ["space-medium-1.wav"] } },
    "enter": { "down": { "medium": ["enter-medium-1.wav"] } }
  }
}
```

- **groups**: `alpha` (letters, numbers, punctuation), `space`, `enter`, `backspace`, `modifier`. Missing groups fall back to `alpha`.
- **down**: takes per strength tier: `soft`, `medium`, `hard`, `slam`. List several takes per tier; Keyslam varies them from key to key. Missing tiers fall back to the nearest tier you provide, with a gain correction.
- **up**: optional key-release takes.
- Schema: [`spec/pack.schema.json`](spec/pack.schema.json).

## How the bundled packs were made

Each bundled pack is cut from one real typing recording. Every strike is detected and paired with its key release. The strikes are then sorted by natural loudness into four strength bands (soft, medium, hard, and slam = the hardest ~8%), and each band is leveled to a consistent target. Every pack is leveled to the same loudness (-28 LUFS), so switching boards doesn't jump in volume.

## Example

[`examples/mx-blue/`](examples/mx-blue/) is Keyslam's full Cherry MX Blue pack, cut from a CC0 recording by EricsSoundschmiede on Freesound.

## Credits (bundled packs)

All source recordings are public domain (CC0) on Freesound. Thank you to the people who recorded and shared them.

| Pack | Kind | Recording | By | License |
| --- | --- | --- | --- | --- |
| MX Blue | Clicky | [Mechanical Keyboard 'Cherry Blue Switches'](https://freesound.org/s/382057/) | EricsSoundschmiede | CC0 1.0 |
| MX Brown | Tactile | [Mechanical Keyboard Cherry MX Brown](https://freesound.org/s/400167/) | majod | CC0 1.0 |
| MX Red | Linear | [Keyboard clicking typing Cherry Red Mechanical](https://freesound.org/s/499773/) | handygaber | CC0 1.0 |
| Topre | Electro-capacitive | [Keyboard Typing 2 (HHKB, Topre)](https://freesound.org/s/546033/) | grcekh | CC0 1.0 |
| Leopold | Tactile | [Mechanical Keyboard Typing (Treble Version)](https://freesound.org/s/450282/) | stu556 | CC0 1.0 |
| WhiteFox | Tactile | [Keyboard Typing 10 (WhiteFox, Mechanical)](https://freesound.org/s/546167/) | grcekh | CC0 1.0 |
| Vintage Technics | Linear, modded | [Keyboard typing sounds: Unidentified Technics keyboard](https://freesound.org/s/665075/) | zrrion | CC0 1.0 |
| Laptop | Butterfly | [Computer keyboard typing and keystrokes - Apple MacBook Pro 2018](https://freesound.org/s/565645/) | khenshom | CC0 1.0 |
| Selectric | Typewriter | [Typewriter, IBM Selectric II](https://freesound.org/s/224012/) | secretmojo | CC0 1.0 |
| Selectric | Typewriter | [Typewriter bell & carriage reset](https://freesound.org/s/345955/) | knufds | CC0 1.0 |
| Olivetti | Typewriter | [typewriter-olivetti-lettra-22.wav](https://freesound.org/s/123344/) | keithpeter | CC0 1.0 |
| Olivetti | Typewriter | [Typewriter ding_near_mono.wav](https://freesound.org/s/406243/) | _stubb | CC0 1.0 |

## License

The example pack's audio is derived from CC0 recordings and released under [CC0 1.0](https://creativecommons.org/publicdomain/zero/1.0/). The spec, schema and this README are CC0 too: use them in any app.

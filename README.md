# Secure Password Generator

A single-file, client-side password and passphrase generator. Everything runs in
the browser — no build step, no dependencies, no network calls, no storage.
Open `Secure-Password-Generator.html` in any modern browser and it works.

Generated values exist only in page memory and are gone the moment the tab is
closed. The tool is built for generating strong credentials for work accounts,
service accounts, databases, API tokens, and anything you need to memorise.

---

## Quick start

1. Open the HTML file in a browser (double-click, or serve it however you like).
2. Pick a **mode** (Simple → High Security, Custom, or Passphrase).
3. Set **QTY** and click **Generate**.
4. Click any masked card to reveal/copy, or use **Copy All** / **Export .txt**.

That's it. No install, no config files.

---

## Security model

| Property | Detail |
|---|---|
| **Randomness** | All randomness comes from `crypto.getRandomValues()` (CSPRNG). `Math.random()` is never used anywhere. |
| **Unbiased selection** | `secureRand()` uses rejection sampling so there is no modulo bias, even for large word pools. |
| **Zero persistence** | No `localStorage`, no `sessionStorage`, no cookies, no server calls. Passwords live only in memory. |
| **Client-side only** | The entire app is one HTML file. Nothing is transmitted or logged. |
| **Integrity hash** | The footer shows a live **SHA-256** of the page's own source. Publish that hash internally so users can verify the file hasn't been tampered with. |
| **Clipboard hygiene** | The clipboard indicator pill lights when a password is copied, can be clicked to clear, and auto-clears after 8 seconds. |

The only outbound requests the file makes are to Google Fonts for the display
typeface. Remove the `<link>` tags in `<head>` if you need a fully air-gapped,
zero-network copy.

---

## Modes

### Fixed character-set modes

These build a password by guaranteeing at least one character from each enabled
class, then filling the rest from the combined pool and shuffling.

| Mode | Length | Character set |
|---|---|---|
| **Simple** | 12 | A–Z, a–z, 0–9, `! - +` |
| **Medium** | 16 | A–Z, a–z, 0–9, `! @ # $ % ^ & * - _ + =` |
| **Complex** | 24 | adds `( ) [ ] { } < > ? / \|` |
| **Extended** | 32 | adds `~ \`` |
| **High Security** | 64 | adds `: ;` — maximum density and length |

### Custom

Full control over the character pool, length (1–256), and structural
constraints.

**Character classes:** Uppercase, Lowercase, Numbers, Symbols (toggle any on/off).

**Constraints:**
- *Starts with letter* — first character is always a letter.
- *No adjacent repeats* — no character appears twice in a row.
- *Min 3 of each class* — guarantees at least 3 chars from each enabled class.

**Built-in presets** (one click sets length, classes, filters, and constraints):

| Preset | Length | Notes |
|---|---|---|
| Service Account (AD Safe) | 28 | CLI-safe, starts with letter, min classes |
| Linux / CLI Safe Secret | 24 | CLI-safe symbols only |
| API Key / Token | 48 | alphanumeric, no symbols |
| Database Password | 24 | CLI-safe, no symbols, min classes |
| NIST SP 800-63B | 20 | non-ambiguous, all classes |
| Microsoft Azure AD | 16 | CLI-safe, min classes |
| AWS Secret Manager | 32 | alphanumeric, CLI-safe |
| WiFi WPA3 | 20 | non-ambiguous, all classes |

### Passphrase

Word-based credentials from the **EFF Large Wordlist** (7,776 words,
CC BY 3.0 US, ~12.92 bits per word). The default is **strict EFF Diceware** —
space-separated, no transforms — which is the recommended mode for any
credential you have to type or remember.

**Passphrase options:**

- **Pool Depth** — 1,000 / 2,000 / 4,000 / 7,776 words. Smaller pools are easier
  to read; the full 7,776-word pool gives the full EFF entropy.
- **Verbosity** — word count per phrase: Default (3–4), Extra (5–7),
  Advanced (7–9), Maximum (10–14).
- **Syntax** — when on, structures output as Subject·Verb·Object or
  Adjective·Noun for more pronounceable phrases (draws from role-tagged pools).
- **Additional separators & transforms** — off = strict Diceware; on = varied
  delimiters and word transforms driven by the Intensity slider.
- **Intensity** — controls how aggressive the transforms and delimiter variety
  get in non-strict mode.

---

## Global filters

Two toggles apply on top of any mode:

- **Non-Ambiguous** — strips visually confusable characters
  (`0 1 O I L l o | / \` space `. : ;`). Good for printed or read-aloud creds.
- **CLI-Safe** — strips shell-hostile characters
  (`? " ' \` $ \ | < > &` and space). Good for passwords pasted into terminals,
  connection strings, and config files.

A warning appears if filters remove every character from the pool.

---

## Output & card metadata

Each generated password is a card showing:

| Field | Meaning |
|---|---|
| **CHAR LEN** | Total character count, including separators. |
| **ENTROPY** | Estimated bits of entropy for that specific value. |
| **TIER** | Strength classification (Moderate / Strong / etc.). |
| **RESIST** | Estimated time to crack at ~1 trillion guesses/second. |

**Card actions:** click a card to copy it; click the eye to reveal a masked
value; hold to peek. A *stale* badge appears on cards when you change settings
after generating, prompting a regenerate.

**Toolbar actions:** Generate, Copy All (copies unmasked values), Clear,
Export .txt (timestamped file with full metadata header), and Mask Output.

`Ctrl + P` / File → Print produces a clean credential sheet.

---

## How entropy is calculated

- **Fixed/Custom modes:** `length × log2(pool size)`. Custom mode subtracts a
  small penalty for constraints (min-classes structure, starts-with-letter lead)
  and shows the deduction inline.
- **Strict EFF passphrase:** exactly `words × log2(7776)` ≈ 12.92 bits/word.
- **Syntax-engine passphrase:** averages the `log2` of each role pool's size.
- **Non-strict passphrase:** pool-size based, with small bonuses for
  capitalisation and numeric suffixes.

Resist-time labels run from "< 1 minute" up to "Practically forever" based on
the bit count.

---

## Keyboard shortcuts

| Shortcut | Action |
|---|---|
| `Ctrl + Enter` | Generate new passwords |
| `Ctrl + Space` | Toggle mask on/off |
| `Ctrl + Shift + Backspace` | Syntax Overdrive — 10 rapid generations, then lock |
| `Escape` | Close any open panel |

---

## SECURED pill commands

Click the **SECURED** pill (top-right), then type within 5 seconds:

| Command | Effect |
|---|---|
| `noclip` | Disable clipboard copy (pill pulses red while active) |
| `clip` | Re-enable clipboard |

---

## Awesome Mode (the fun part)

A green-on-black "Cryptographic Flavor Generator" skin with animated Matrix rain,
hidden behind Corp mode. By design, Corp mode leaks no hint that it exists — so
the entry points are documented here for maintainers.

### Entering & exiting

| Method | Effect |
|---|---|
| `Ctrl + Alt + Shift` | Toggle Awesome Mode on/off — the keyboard route in and out |
| Type `awesome` | Into the SECURED pill (within 5s of clicking it) to switch on |
| Type `corp` | Into the SECURED pill to return to the standard Corp view |

The `Ctrl + Alt + Shift` shortcut is intentionally absent from Corp mode's in-app
About panel — it only surfaces in the reference once you're *already* in Awesome
Mode. It's listed here because this README is the full maintainer reference.

### Flavor System

In passphrase mode, Awesome Mode adds 16 themed word pools you can blend into
your passphrases: D&D, Gibson, Star Trek, Star Wars, Cosmic Horror, Mythology,
Linux Kernel, Corp Speak, Pirate, Dune, Dark Souls, Latin, Tolkien, Victorian,
Arthurian, and Hacker.

- **Conflicts** — incompatible flavor pairs trigger a conflict flash; click it to
  generate a *Mutation* card blending both pools.
- **Synergies** — compatible pairs trigger a synergy flash; click it for a
  *Synergy Bonus* card with dual output.
- **Intensity tiers** drive the rain speed/colour: stable → volatile → chaotic →
  breach.
- **Insane Mode** — activate all 16 flavors at once for a special skin; the rain
  switches to INSANE/MODE characters.

### Generate button morphs

In Awesome Mode the **Generate** button's label changes to reflect flavor state:

- **Single flavor** — shows that theme's action word.
- **Multiple flavors** — cycles through them.
- **Conflict active** — shows `CONFLICT`.
- **Synergy active** — shows `SYNERGIZE`.
- **Both active** — a glitch cycle between states.

### Other Awesome-only behavior

  Mutation, and Synergy Bonus cards through its 10 generations before locking.
- **Lore egg** — hover the Status chip for ~2 seconds at **Maximum** intensity to
  surface a lore one-liner.

This is cosmetic flavour layered on top of the same CSPRNG engine — it does not
weaken the underlying entropy.

---

## Acceptable use

- Generate credentials for work accounts, service accounts, and internal systems.
- Store generated values only in your approved password manager.
- Never share passwords over unencrypted channels (email, chat).
- Verify the footer SHA-256 against the published value before use on sensitive
  systems.
- Always use the copy distributed from your official internal source.

Questions: contact your IT Security team via the internal helpdesk.

---

## Technical notes

- **Single file.** All HTML, CSS, and JS (including the full 7,776-word EFF
  wordlist) are inline. Nothing else is required to run it.
- **No dependencies / no build.** Pure vanilla JS; works offline once fonts are
  cached or the font links are removed.
- **Browser support.** Any modern browser with the Web Crypto API
  (`crypto.getRandomValues`, `crypto.subtle`).
- **Wordlist source.** EFF Large Wordlist — eff.org/dice — CC BY 3.0 US.

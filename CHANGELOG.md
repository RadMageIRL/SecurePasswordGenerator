# Changelog

Every version of Secure Password Generator and what changed in it, newest first. Dates are the day I released each one.

## v1.1.2 (2026-11-17)

Fixed
- The regenerate button on a single card now works in Passphrase and Custom. Generate reuses the cards already on the page, and each card's button kept the mode it was first built in. After switching to Passphrase or Custom from another mode, the button called the wrong generator, hit an error and left the card unchanged. It now reads the mode at the moment it is pressed.

Changed
- The footer and the page's structured data show version 1.1.2.
- The README shows current screenshots of the generator, one in the standard look and one in Awesome Mode.

The random functions, the word lists and the password modes are unchanged.

## v1.1.1 (2026-11-15)

Added
- Word Count, Separator and Add numbers to words now work at every passphrase verbosity: Default, Extra, Advanced and Maximum as well as Simple. Before, only Simple had them.
- Word Count lists each count in the verbosity's range (3 or 4 for Default, 5 to 7 for Extra, 7 to 9 for Advanced, 10 to 14 for Maximum), plus Random, which picks a count for each phrase the way earlier versions did. Random is selected by default, so a passphrase from an untouched page is the same as in v1.0.1. Simple has no Random and still starts at 3 words. Each verbosity keeps its own choice.
- Separator and Add numbers to words also apply with Syntax on.
- Share and Buy me a coffee links in the RadMage Labs bar at the top and in the footer. Share always passes https://radmagelabs.com/password/, even from a downloaded copy. Where the device has a share sheet it opens that; otherwise, or if the sheet fails, it copies the link. Copying the link stops the clipboard's 8-second auto-clear, which would otherwise wipe the link. The coffee link opens in a new tab, so generated passwords stay in this one.

Changed
- Mask Output is off when the page opens, so generated values show straight away. Turn it on, or press Ctrl+Space, to hide them.
- With Additional separators & transforms on, the Separator row is hidden, because the toggle picks its own separators. Add numbers to words still applies, and its digits go on before any transform.
- With Add numbers to words on, entropy counts log2(100), about 6.64 bits, for each word's number at every verbosity. With transforms on too, that replaces the old 6-bit estimate for a number at the end of a word.
- The EFF status strip names the separators in use instead of always saying space-separated.
- The footer and the page's structured data show version 1.1.1.
- The README, the docs and the in-page Reference describe the options at every verbosity and say Mask Output starts off. The README links this changelog and the coffee page.

The random functions, the word lists and the password modes are unchanged.

## v1.0.1 (2026-10-26)

Changed
- Corp Mode no longer has an Intensity control. The slider and the Status chip beside it appear only in Awesome Mode, where they go with the flavors.
- In Corp Mode, passphrases always use the default intensity (Low), the same as a fresh page load before this release. With Additional separators & transforms on, that means one separator per phrase from - _ and . .
- A .txt export made in Corp Mode is titled "Secure Password Generator Export" and carries no Awesome Mode wording: the Flavor Engine title and the Intensity, Conflicts, Synergies, Flavors and Dual Output lines appear only in exports made in Awesome Mode.
- Corp Mode's About panel no longer mentions Intensity, and the separators toggle tooltip says "word transforms".
- The README and the docs list Intensity under Awesome Mode.

The generation engine is unchanged.

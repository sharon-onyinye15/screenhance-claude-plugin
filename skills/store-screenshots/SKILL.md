---
name: store-screenshots
description: Make App Store and Google Play screenshot sets, product films, device mockups and launch artwork from the user's real app screenshots with Screenhance. Use when the user asks for store screenshots, App Store or Play Store images, app mockups, a launch kit, OG or social images for an app, or wants to refresh screenshots for a release.
---

# Store screenshots and launch artwork with Screenhance

Screenhance turns real app screenshots into designed store screenshots, motion, mockups and launch graphics. It handles layout, device frames and exact store sizes. You bring the screenshots and the words.

## 1. Check the account

Call `get_screenhance_account`. It reports the remaining exports and whether exports carry a watermark. Mention a watermark before exporting. Do not promote plans or link to purchases.

## 2. Bring in the real screenshots

Never invent, generate or edit the app's screenshots yourself.

- **In chat:** call `create_screenshot_upload` and give the user the `uploadPage` link to drop their screenshots. When they say they are done, call `get_screenshot_upload` with the `uploadId` and use the returned IDs. An empty list means nothing has arrived yet.
- **In Claude Code:** look for existing screenshots first: `fastlane/screenshots/<locale>/`, a `screenshots/` folder, design exports. To capture fresh ones, ask first, then:
  - iOS Simulator: `xcrun simctl status_bar booted override --time 9:41 --batteryState charged --batteryLevel 100`, then `xcrun simctl io booted screenshot screens/01.png`
  - Android: `adb exec-out screencap -p > screens/01.png`

  Then call `create_screenshot_upload` and send each file to its `uploadUrl`: `curl -sS -F file=@screens/01.png "<uploadUrl>"`. Each response holds the screenshot ID. Files can be up to 4 MB; save a bigger PNG as JPEG first (macOS: `sips -s format jpeg -s formatOptions 90 big.png --out big.jpg`).
- Pick 3 to 6 screens that tell the story in order: the core value first, then the features that make people stay.

## 3. Write the brief

- `appName` and a short `description` from the user, or in Claude Code from the repo: README, `Info.plist` display name, `app.json`, `strings.xml`, `fastlane/metadata`.
- `surfaces`: `appstore` and `playstore` for a mobile app's store listings. Add `og`, `animation`, `mockup_solo`, `mockup_scene` or social sizes only when the user wants them.
- Copy: captions of 2 to 6 words, benefit first ("Plan your week in minutes", not "Calendar screen"). Never invent ratings, rankings, user counts, awards or press quotes.
- `brandStyle` and colors only when the user or the project states them (for example `AccentColor` in the asset catalog).

## 4. Compare directions, then build

1. `create_launch_kit` with a new `requestKey`; reuse the same key if you retry. It saves three ranked directions and opens the studio.
2. Show them with `show_launch_kit`. Explain the recommended pick and any crop or text cautions, and let the user choose.
3. `edit_launch_kit` with the chosen `directionId` builds every requested output.
4. For changes, call `get_launch_kit` for the current revision and element IDs, then make targeted `edit_launch_kit` edits.
5. `generate_studio_previews` and `get_studio_preview` show watermarked previews without using an export.

## 5. Export

1. `preview_launch_render` with the same revision and selection. Tell the user what will be exported, whether it is watermarked and how many exports remain.
2. Only after the user agrees, call `render_launch_kit`, then `get_launch_render` until the job completes. Each request uses one export; failed exports are refunded.
3. Share the download links. They last one hour; call `get_launch_render` again for fresh links.

## 6. Save into the project (Claude Code)

When the user wants the files in their repo:

- Download each file with `curl -sSL -o <file> "<url>"`.
- App Store: put the PNGs in `fastlane/screenshots/<locale>/`, for example `fastlane/screenshots/en-US/01.png`. `fastlane deliver` uploads every image in a locale folder and reads the device from the pixel size.
- Google Play: put them in `fastlane/metadata/android/<locale>/images/phoneScreenshots/`, for example `.../en-US/images/phoneScreenshots/1.png`.
- Ask before deleting or replacing older screenshots in those folders.

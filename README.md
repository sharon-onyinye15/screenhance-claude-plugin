![Screenhance](assets/icon.png)

# Screenhance for Claude

Screenhance turns your real app screenshots into designed App Store and Google Play screenshots, product films, scene and device mockups, Open Graph images and social artwork. Claude compares three creative directions with you, applies your edits, shows watermarked previews and exports native-size files.

## Use it

1. Install the plugin and connect Screenhance from its **Connectors** tab (or run `/mcp` in Claude Code). You sign in to your Screenhance account and approve access once. You can create a free Screenhance account during sign-in.
2. Ask for what you need, for example "Make App Store and Google Play screenshots for my app".
3. Add your screenshots. In chat, Claude gives you a private upload link that works for one hour. In Claude Code, Claude can upload screenshots from your project or simulator for you.
4. Pick a direction, ask for changes, then ask Claude to export. Exports use your Screenhance plan's export allowance. In Claude Code, Claude can save App Store screenshots into `fastlane/screenshots` and Play screenshots into `fastlane/metadata/android`.

## What the plugin contains

- `.mcp.json`: one remote connector, the Screenhance server at `https://app.screenhance.com/api/mcp` (OAuth sign-in).
- `skills/store-screenshots`: instructions that teach Claude the workflow above. There are no hooks, scripts or local servers.

## Data and what runs

- **Sent to Screenhance (app.screenhance.com):** the screenshots you add and the details you give Claude for your designs, such as app name, description, captions, colors and edit requests. Claude never sends your chat history.
- **Downloaded:** previews and finished files come from the time-limited links Screenhance returns, which point to Screenhance's private file storage (hosted on Supabase).
- **Commands in Claude Code:** when you ask, Claude may run `curl` to upload screenshots to your private Screenhance upload link and to download finished files, `xcrun simctl` or `adb` to capture simulator or device screenshots, and `sips` to convert large images. Claude asks before capturing screenshots or replacing files in your project.
- **Storage:** screenshots, projects and exports stay in your Screenhance account until you delete them. Disconnect at any time from Claude or from Screenhance under Connected apps.

Read the [privacy policy](https://app.screenhance.com/privacy) and [terms](https://app.screenhance.com/terms). Help: support@screenhance.com.

## License

Copyright (c) 2026 Screenhance. All rights reserved. You may install and use this plugin with a Screenhance account; see [LICENSE](LICENSE).

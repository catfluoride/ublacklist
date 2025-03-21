# uBlacklist

[简体中文](README.zh-CN.md)

Blocks specific sites from appearing in Google search results

[Chrome Web Store](https://chrome.google.com/webstore/detail/ublacklist/pncfbmialoiaghdehhbnbhkkgmjanfhe) / [Firefox Add-ons](https://addons.mozilla.org/en-US/firefox/addon/ublacklist/) / [App Store](https://apps.apple.com/us/app/ublacklist-for-safari/id1547912640) (for macOS and iOS, thanks to [Group-Leafy](https://github.com/HoneyLuka/uBlacklist/tree/safari-port/safari-project))

## Description

This extension prevents the sites you specify from appearing in Google search results.

You can add rules on search result pages, or on sites to be blocked by clicking the toolbar icon. Rules can be specified either by [match patterns](https://developer.mozilla.org/en-us/docs/mozilla/add-ons/webextensions/match_patterns) (e.g. `*://*.example.com/*`) or by [regular expressions](https://developer.mozilla.org/en-us/docs/web/javascript/guide/regular_expressions) (e.g. `/example\.(net|org)/`).

You can synchronize rulesets across devices via cloud storage. At the moment, Google Drive and Dropbox are supported.

You can also subscribe to public rulesets. Some subscriptions are listed on the website:
https://iorate.github.io/ublacklist/subscriptions

## Supported search engines

This extension is available in the below search engines.

|              | Web                | Images             | Videos             | News               |
| ------------ | ------------------ | ------------------ | ------------------ | ------------------ |
| Google       | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Bing         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Brave \*3    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| DuckDuckGo   | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Ecosia       | :heavy_check_mark: |                    |                    |                    |
| Kagi         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Qwant        | :heavy_check_mark: | :heavy_check_mark: | \*1                | :heavy_check_mark: |
| SearX \*2    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |
| Startpage    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |
| Yahoo! JAPAN | :heavy_check_mark: |                    |                    |                    |
| Yandex       | :heavy_check_mark: |                    |                    |                    |

\*1 Only if "Always play videos on Qwant.com" is turned off<br>
\*2 Only certain public instances are supported. If you want to add support for your own SearX/SearXNG search engine, edit `src/common/search-engines.ts` and build the extension manually<br>
\*3 Due to the difficulty in obtaining the full URLs of image links, this extension does not support per-subdomain blocking in Brave Image Search. (For example, if the link is to "www.example.com", it will block the entire "example.com").

## For subscription providers

To publish a ruleset as a subscription, place a ruleset file encoded in UTF-8 on a suitable HTTP(S) server, and publish the URL. Here is an example hosted on GitHub:<br>
https://raw.githubusercontent.com/iorate/ublacklist-example-subscription/master/uBlacklist.txt

You can prepend YAML frontmatter to your ruleset. It is recommended that you set the `name` variable.

```
---
name: Your ruleset name
---
*://*.example.com/*
```

In uBlacklist >=6.6.0 for _Chrome_, subscription links are available. To add a subscription with `url`, the following URL can be used as a shortcut to the options page:

```
https://iorate.github.io/ublacklist/subscribe?url={urlEncode(url)}
```

For the above example:<br>
https://iorate.github.io/ublacklist/subscribe?url=https%3A%2F%2Fraw.githubusercontent.com%2Fiorate%2Fublacklist-example-subscription%2Fmaster%2FuBlacklist.txt

## For developers

### Build

To build this extension, [pnpm](https://pnpm.io/)>=9.7.0 or [corepack](https://github.com/nodejs/corepack) (currently distributed with Node.js) is required.

```shell
# If you use corepack
# corepack enable

git clone https://github.com/catfluoride/ublacklist.git

cd ublacklist

pnpm install

# Usage: pnpm build [--browser BROWSER] [--version VERSION] [--debug] [--watch]
pnpm build
```

Before opening a pull request, you should make sure that `pnpm check` passes.

```shell
pnpm check

# Some lint errors can be fixed automatically
pnpm fix
```

**NOTE:** The API keys and secrets for the sync feature are not included in this repository. To develop the sync feature, set your own API keys and secrets in the `.env` file.

```
DROPBOX_API_KEY=...
DROPBOX_API_SECRET=...
GOOGLE_DRIVE_API_KEY=...
GOOGLE_DRIVE_API_SECRET=...
```

### Locale

To add a locale,

1. Determine an ISO language code such as `en` referring to [kLanguageInfoTable](https://src.chromium.org/viewvc/chrome/trunk/src/third_party/cld/languages/internal/languages.cc).
1. Copy `src/locales/en/messages.json.ts` to `src/locales/${languageCode}/messages.json.ts` and translate entries.
1. Open `src/scripts/dayjs-locales.ts` and import the dayjs locale.
1. To localize description and screenshots on web stores, create `web-store-assets/${languageCode}/` and add files.
   - Screenshot localization is available only on Chrome Web Store.
   - Screenshots should be 1280x800.

## Author

[iorate](https://github.com/iorate) ([Twitter](https://twitter.com/iorate))

## License

uBlacklist is licensed under [MIT License](LICENSE.txt).

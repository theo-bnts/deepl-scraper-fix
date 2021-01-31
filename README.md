# deepl-scraper

Scrape data from DeepL translator without applying for the paid authenticated API, using Puppeteer, Chrome browser's headless API.

## Getting started

### Prerequisites

- NodeJS
- NPM
- Yarn

### Install

From [npm](https://www.npmjs.com/package/deepl-scraper)

`yarn add deepl-scraper`

or

`npm i deepl-scraper --save`

#### Additional steps for `puppeteer` on Linux

```bash
sudo apt install libnss3-dev libxss1
sudo sysctl -w kernel.unprivileged_userns_clone=1
```

### Usage

```js
const { translate, getSupportedLanguages, quit } = require('deepl-scraper');

translate(sentence, source, target).then(console.log).catch(console.error);
```

#### Parameters

- `sentence` *string* - Word/sentence to be translated
- `source` *string* (optional) - Word/sentence original language
<br>Default : `auto`
- `target` *string* - Language for word/sentence to be translated

#### Supported languages

The module doesn't store languages and will always support DeepL's languages list without required update.

IETF tags are used as language format.

Get arrays of supported source and target languages using the following code :
```js
getSupportedLanguages().then(console.log);
```

```json
{
    "sourceLanguages": [
        "en",
        "de",
        "fr",
        "es",
        "pt",
        "pt",
        "it",
        "nl",
        "pl",
        "ru",
        "ja",
        "zh"
    ],
    "targetLanguages": [
        "en-US",
        "en-GB",
        "de-DE",
        "fr-FR",
        "es-ES",
        "pt-BR",
        "it-IT",
        "nl-NL",
        "pl-PL",
        "ru-RU",
        "ja-JA",
        "zh-ZH"
    ]
}

```

#### Quitting

Since this module uses a headless browser, it won't quit as long as your main script is ronning or until you quit it using the following code :

```js
quit()
```

#### Error handling

- `INVALID_SOURCE_LANGUAGE`
- `INVALID_TARGET_LANGUAGE`
- `UNSUPPORTED_SOURCE_LANGUAGE`
- `UNSUPPORTED_TARGET_LANGUAGE`

### Examples

#### Translate from defined language

```js
translate('hello', 'en', 'fr-FR').then(console.log);
```

```json
{
    "source": {
        "lang": "en",
        "sentence": "hello"
    },
    "target": {
        "lang": "fr",
        "sentences": [
            "Bonjour,",
            "Bonjour",
            "bonjour",
            "hello",
            "h"
        ],
        "translation": "hello"
    }
}
```

#### Translate from auto-detected language

(a single word's language is usually more difficult to detect)

```js
// Either
translate('hello', 'auto', 'fr-FR').then(console.log);
// Or
translate('hello', undefined, 'fr-FR').then(console.log);
```

```json
{
    "source": {
        "lang": "en",
        "confident": false,
        "sentence": "hello"
    },
    "target": {
        "lang": "fr",
        "sentences": [
            "Bonjour,",
            "Bonjour",
            "bonjour",
            "hello",
            "h"
        ],
        "translation": "hello"
    }
}

```

(a sentence's language is usually more efficient)

```js
translate('hey, what\'s up ?', 'auto', 'fr-FR').then(console.log);
```

```json
{
    "source": {
        "lang": "en",
        "confident": false,
        "sentence": "hey, what's up ?"
    },
    "target": {
        "lang": "fr",
        "sentences": [
            "hey, quoi de neuf ?",
            "Hé, qu'est-ce qu'il y a ?",
            "h",
            "Hé, quoi de neuf ?",
            "Hé, qu'est-ce qu'il y a ?"
        ],
        "translation": "Hé, qu'est-ce qu'il y a ?"
    }
}
```

## Planned features

- Get word-by-word definitions, quotes and synonyms
- Bypass maximum text length

## FAQ

**Why using Puppeteer instead of HTTP requests ?**
<br>Fucking rate limit error always showing up, whatever you do.

## Changelog

* `1.0.0` (2019-05-06) • Initial release
* `1.0.x` (2019-05-11)
	<br>`1.0.1`
	- Added examples title in `README.md`
	- Fixed Promise never resolving while alt sentences not available
	<br>`1.0.2`
	- Retry when browser error
	<br>`1.0.3`
	- Fixed retry
	- Fixed changelog version typo
	<br>`1.0.4`
	- Close page after translation
* `1.0.5` (2019-06-27)
	- Added `getSupportedLanguages` method
	- Improved translation complete detection
	- Fixed cannot launch new browser when previously closed
	- Fixed incompatibility with multi-requests translation (#1)
* `1.0.6` (2019-07-05)
	- Fixed auto/non-auto parameter
	- Fixed translation complete detection *(again)*
* `1.0.7` (2019-07-05)
	- Fixed translation complete detection *(again !)*
* `1.0.9` (2020-07-10)
	- Fixed supported languages detection & selection
	- Improved translation completion detection & result parsing
	- Replaced deprecated request dependency by axios dependency
	- Replaced JSDOM dependency by RegExp processing
	- Replaced `quit` parameter from `translate` method to new `quit` method
	- Refactored using async/await
* `1.0.10` (2020-08-21) • Actually remove jsdom NPM dependency
* `1.0.11` (2020-08-24)
    - Fixed & improved supported languages parsing
    - Implemented `UNSUPPORTED_SOURCE_LANGUAGE` & `UNSUPPORTED_TARGET_LANGUAGE` errors
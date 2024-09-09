# 🦄 @jacobwolf/is-emoji-supported

![npm](https://img.shields.io/npm/v/@jacobwolf/is-emoji-supported)
![npm](https://img.shields.io/npm/dm/@jacobwolf/is-emoji-supported)

![No dependency](https://img.shields.io/badge/dependencies-none-blue.svg)
[![license: MIT](https://img.shields.io/badge/license-MIT-brightgreen.svg)](https://opensource.org/licenses/MIT)

A lightweight library to detect emoji support in browsers and provide fallbacks.

> This is a maintained fork of the original [koala-interactive/is-emoji-supported](https://github.com/koala-interactive/is-emoji-supported) package. It fixes the original package's source map issues and adds improvements in emoji detection and fallback handling.

## Features

- Detect emoji support across different browsers and devices
- Provide fallback options for unsupported emojis
- Lightweight with zero dependencies
- Customizable caching for improved performance

## 📖 Table of Contents

- [🦄 @jacobwolf/is-emoji-supported](#-jacobwolfis-emoji-supported)
  - [Features](#features)
  - [📖 Table of Contents](#-table-of-contents)
  - [🚀 Installation](#-installation)
  - [🖥️ How to use](#️-how-to-use)
    - [Basic usage](#basic-usage)
    - [Usage with your own cache handler](#usage-with-your-own-cache-handler)
    - [Fallback to images](#fallback-to-images)
  - [⏳ How to test](#-how-to-test)
  - [🤝 How to contribute](#-how-to-contribute)
  - [License](#license)

## 🚀 Installation

Install with [yarn](https://yarnpkg.com):

    $ yarn add @jacobwolf/is-emoji-supported

Install using [npm](https://npmjs.org):

    $ npm install @jacobwolf/is-emoji-supported

Install using [pnpm](https://pnpm.io):

    $ pnpm add @jacobwolf/is-emoji-supported

Install using [bun](https://bun.sh):

    $ bun add @jacobwolf/is-emoji-supported

## 🖥️ How to use

### Basic usage

The most basic usage is to use the function directly to detect is the current device support the emoji.

```ts
import { isEmojiSupported } from "@jacobwolf/is-emoji-supported";

if (isEmojiSupported("🦄")) {
  alert("Houra 🦄 is supported");
} else {
  alert("No support for unicorn emoji yet");
}
```

### Usage with your own cache handler

This library is doing pixel comparison to determine if an emoji is supported. This check can be slow so there is a memory cache implemented.
For some reasons you may want to use your own cache implementation to store the result in either localStorage, IndexedDB or anything else for persistent cache.
You only need to match the [`Map`](https://developer.mozilla.org/fr/docs/Web/JavaScript/Reference/Objets_globaux/Map) interface.

```ts
import { setCacheHandler } from "@jacobwolf/is-emoji-supported";

const key = "emoji-cache";
const cache = JSON.parse(localStorage.getItem(key) || {});

setCacheHandler({
  has: (unicode: string) => unicode in cache,
  get: (unicode: string) => cache[unicode],
  set: (unicode: string, supported: boolean) => {
    cache[unicode] = supported;
    localStorage.setItem(key, JSON.stringify(cache));
  },
});
```

### Fallback to images

In most of the cases, you will want to fallback to images to handle unsupported emojis. The best way for this is to build an object with a fallback to all supported images.
You can build your own or use the one given by [JoyPixel](https://www.joypixels.com/), [Twemoji](https://twemoji.twitter.com/) or others services.

```jsx
import React from 'react';
import {isEmojiSupported} from '@jacobwolf/is-emoji-supported';

const emojiMap = {
  '🦄': {
    alt: 'unicorn',
    src: '/images/unicorn.png'
  },
  ...
};

export const Emoji = ({ unicode }) => {
  const attrs = emojiMap[unicode];

  return !attrs ? null : isEmojiSupported(unicode) ? (
    <span role="img" aria-label={attrs.alt}>
      {unicode}
    </span>
  ) : (
    <img {...attrs} />
  );
};
```

## ⏳ How to test

    $ npm test

## 🤝 How to contribute

This project is a fork of the original [koala-interactive/is-emoji-supported](https://github.com/koala-interactive/is-emoji-supported) package.

- Fork the project on GitHub
- Clone your forked repository:

      $ git clone https://github.com/your-username/is-emoji-supported.git
      $ cd is-emoji-supported

- Install dependencies:

      $ npm install

- Create a new branch from main:

      $ git checkout -b contribution/fix/your-github-username

  OR

      $ git checkout -b contribution/improvement/your-github-username

- Make your changes and commit them with clear, descriptive messages
- Add or update tests as necessary
- Ensure all tests pass:

      $ npm test

- Push your changes to your fork:

      $ git push origin your-branch-name

- Open a pull request on the original repository

## License

Original work Copyright (c) 2023 [Koala-Interactive](https://koala-interactive.com/).

Modified work Copyright (c) 2024 [Jacob Wolf](https://twitter.com/jacobwolf).

This project is [MIT](./LICENSE) licensed.

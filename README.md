# x-client-transaction-id-js

**x-client-transaction-id-js** is a lightweight utility library for generating the `X-Client-Transaction-ID` header in browser environments—no backend server required. This makes it perfect for browser extensions and userscripts where secure, dynamic identification is needed.

---

## Features

* 🔐 No backend server needed
* ⚙️ Pure JavaScript with async/await
* 🧠 Automatically extracts required animation frames
* 📦 Saves frames locally to speed up future generation
* 🔁 Re-usable for multiple API endpoints

---

## Installation

You can directly include it in your browser script or extension:

```js
<script src="x-client-transaction-id.js"></script>
```

Or copy the source code into your extension background/content script.

---

## How It Works

The Transaction ID is generated using:

* HTTP method (e.g., GET)
* API endpoint path
* Twitter meta tag (as key source)
* Frame animation data extracted from Twitter's loading animation
* Time-based and hashed values

---

## Quick Start

```js
// Start generating transaction ID
await generateTID();
```

Under the hood, this calls the following flow:

```js
const method = "GET";
const path = fetchApiURL();
const key = await getKey();
const keyBytes = getKeyBytes(key);
const animationKey = getAnimationKey(keyBytes);
const xTID = await getTransactionID(method, path, key, keyBytes, animationKey);
console.log("Generated Transaction ID:", xTID);
```

---

## Functions

### `generateTID()`

Main entry function. Handles initialization and calls the other steps.

### `fetchApiURL()`

Returns the target API endpoint.

### `getKey()`

Extracts the key from Twitter meta tag:

```js
document.querySelector('meta[name="twitter-site-verification"]').getAttribute("content")
```

### `getKeyBytes(key)`

Decodes Base64 key into byte array.

### `getFrames()`

Parses and restores saved animation frames from `localStorage`.

### `get2DArray(keyBytes)`

Builds a 2D array of animation frame coordinates using SVG path data.

### `getAnimationKey(keyBytes)`

Computes a hashed animation key using the keyBytes and frame data.

### `getTransactionID(...)`

Final function that assembles the transaction ID using:

* Method
* Path
* Key & KeyBytes
* Time (in seconds)
* Hashed data

Returns a Base64-encoded string.

---

## Example Output

```
Generated Transaction ID: Ax3eIvu2znA3w... (truncated)
```

---

## Notes

* Ensure the Twitter DOM is fully loaded before triggering.
* Works for GraphQL endpoints, tested with `/i/api/graphql/...`
* May require updates if Twitter changes their animation logic or key format.

---

## License

MIT

---

## Contribution

Pull requests are welcome. This repo is designed for developers needing client-side transaction IDs, especially when Twitter's public endpoints require one.

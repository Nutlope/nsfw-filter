<div align="center">
  <div>
    <h1 align="center">NSFW Filter</h1>
  </div>
	<p>An npm library that helps filter out inappropriate images using AI.

<a href="https://www.npmjs.com/package/nsfw-filter"><img src="https://img.shields.io/npm/v/nsfw-filter" alt="Current version"></a>

</div>

---

## Installation

`npm i nsfw-filter`

## Usage

```js
import NSFWFilter from 'nsfw-filter';

const isSafe = await NSFWFilter.isSafe(image);
```

### Full example

```js
import { useState } from 'react';
import NSFWFilter from 'nsfw-filter';

function ImageUploader() {
  const [imageUrl, setImageUrl] = useState('');

  const handleImageUpload = async (event) => {
    const file = event.target.files[0];

    // Check to see if the image is appropriate
    const isSafe = await NSFWFilter.isSafe(file);
    if (!isSafe) return 'Image is not appropriate';

    // Process the image if it is safe
    if (file) {
      const reader = new FileReader();
      reader.onloadend = () => {
        setImageUrl(reader.result);
      };
      reader.readAsDataURL(file);
    }
  };

  return (
    <div>
      <input type="file" onChange={handleImageUpload} />
      {imageUrl && <img src={imageUrl} alt="Uploaded" />}
    </div>
  );
}

export default ImageUploader;
```

### Real world usage

`nsfw-filter` is currently used in production to process hundreds of thousands of images for a popular image restoration service called <a href="https://www.restorephotos.io/">restorePhotos</a>. It helps prevent people from uploading inappropriate pictures. [See how it's used here.](https://github.com/Nutlope/restorePhotos/blob/main/pages/restore.tsx#L50)

## Using in a browser environment with vite

you can polyfill the core node modules used in nsfw-filter for browser compatibility such as 

["path", "stream", "assert", "events", "zlib", "util", "buffer"]
more on this: https://vitejs.dev/guide/troubleshooting.html

install `vite-plugin-node-polyfills` with npm: https://www.npmjs.com/package/vite-plugin-node-polyfills
in `vite.config.js`

```js

import { defineConfig } from "vite";
import react from "@vitejs/plugin-react-swc";
import { nodePolyfills } from "vite-plugin-node-polyfills";
import path from "path";
export default defineConfig({
  plugins: [
    nodePolyfills({
      // To add only specific polyfills, add them here. If no option is passed, adds all polyfills
      include: ["path", "stream", "assert", "events", "zlib", "util", "buffer"],
    }),
    react(),
  ],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src"),
      "~": path.resolve(__dirname, "./"),
    },
  },
});

```
bonus: you can polyfill path to support path aliases for vite when you deploy your could on the cloud for example vercel.
so you can use `@`
`import { cn } from "@/lib/utils"`
disclaimer: using nsfw-filter on the browser will dramatically increase your build size.

## How it works

This library uses both Tensorflow.js, an OSS library for machine learning models, and `nsfwjs` to predict whether a given image is NSFW (Not Safe For Work).

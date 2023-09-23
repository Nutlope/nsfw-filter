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

`nsfw-filter` is currently used in production to process hundreds of thousands of images for a popular image restoration service called <a href="https://www.restorephotos.io/">restorePhotos</a>. It helps prevent people from uploading inappropriate pictures.

## How it works

This library uses both Tensorflow.js, an OSS library for machine learning models, and `nsfwjs` to predict whether a given image is NSFW (Not Safe For Work).

It's currently being used in RestorePhotos, which is a popular open source projects with 250k monthly visitors. [See how it's used here.](https://github.com/Nutlope/restorePhotos/blob/main/pages/restore.tsx#L45)
